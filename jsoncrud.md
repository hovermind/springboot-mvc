## JsonCrudApplication
```
@SpringBootApplication(exclude = ErrorMvcAutoConfiguration.class)
public class JsonCrudApplication extends SpringBootServletInitializer {

	/**
	 * Main method - application entry point
	 * 
	 * @param args
	 *            String command line arguments
	 */
	public static void main(String[] args) {

		ApplicationContext ctx = SpringApplication.run(JsonCrudApplication.class, args);
	}

	/**
	 * {@inheritDoc}}
	 */
	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
		return builder.sources(JsonCrudApplication.class);
	}
}

```

## Exception
```
public class JsonCrudException extends Exception {

	private static final long serialVersionUID = 1L;

	public JsonCrudException(String message, Throwable cause) {
		super(message, cause);
	}

	public JsonCrudException(String message) {
		super(message);
	}
}
```

## Repository
### Inferface
```
public interface IJsonCrudRepository<T, I extends IJcFileInfo<T>> {

	public void create(T bean) throws JsonCrudException;

	public T read(String fileUri, Class<T> clazz) throws JsonCrudException;

	public List<T> readAll(Class<T> clazz) throws JsonCrudException;

	public void update(T bean) throws JsonCrudException;

	public void delete(T bean) throws JsonCrudException;
}
```

### Repo
```
@Repository("jsonCrudRepository")
public class JsonCrudRepository<T, I extends IJcFileInfo<T>> implements IJsonCrudRepository<T, I> {

	protected final Logger jcLogger = LoggerFactory.getLogger(this.getClass());

	@Autowired
	private IJcJsonHelper<T> jsonHelper;

	@Autowired
	private I fileInfo;
	
	@Autowired
	private ServletContext servletContext;

	@Override
	public void create(T bean) throws JsonCrudException {

		String fileUri = fileInfo.getUri(bean);

		if (JcFileUtil.exists(fileUri)) { // duplicate if file already exist
			throw new JsonCrudException("Duplicate file.");
		}

		try {

			JcFileUtil.create(fileUri);

			jsonHelper.writeToJsonFile(fileUri, bean);

		} catch (Exception e) {

			throw new JsonCrudException(e.getMessage());
		}
	}

	@Override
	public T read(String fileUri, Class<T> clazz) throws JsonCrudException {

		if (!JcFileUtil.exists(fileUri)) {
			throw new JsonCrudException("File not found.");
		}

		try {

			return jsonHelper.readFromJsonFile(fileUri, clazz);

		} catch (Exception e) {

			throw new JsonCrudException(e.getMessage());
		}
	}

	@Override
	public List<T> readAll(Class<T> clazz) throws JsonCrudException {
		
		String jsonFolderLocation = fileInfo.getJsonFolderLocation();
		String rootFolder = JcFileUtil.getRealFolderPath(jsonFolderLocation, servletContext);

		Collection<File> files = JcFileUtil.getFileList(rootFolder);

		List<T> list = new ArrayList<T>();

		try {

			for (File file : files) {

				list.add(jsonHelper.readFromJsonFile(file, clazz));
			}

		} catch (Exception e) {

			throw new JsonCrudException(e.getMessage());
		}

		return list;
	}

	@Override
	public void update(T bean) throws JsonCrudException {

		String fileUri = fileInfo.getUri(bean);

		if (!JcFileUtil.exists(fileUri)) {
			throw new JsonCrudException("File not found.");
		}

		try {

			JcFileUtil.eraseContent(fileUri);
			jsonHelper.writeToJsonFile(fileUri, bean);

		} catch (Exception e) {

			throw new JsonCrudException(e.getMessage());
		}
	}

	@Override
	public void delete(T bean) throws JsonCrudException {

		String fileUri = fileInfo.getUri(bean);

		if (!JcFileUtil.exists(fileUri)) {
			throw new JsonCrudException("File not found.");
		}

		try {
			JcFileUtil.delete(fileUri);
		} catch (IOException e) {
			throw new JsonCrudException(e.getMessage());
		}
	}


}
```

## Util
### JcFileUtil
```
public class JcFileUtil {

	public static void write(String fileUri, String jsonString) throws IOException {

		write(fileUri, jsonString, true);

	}

	public static void write(String fileUri, String jsonString, boolean append) throws IOException {

		File file = new File(fileUri);

		try (FileWriter writer = new FileWriter(file, append)) {
			writer.write(jsonString);
		}

	}

	public static String read(String fileUri) throws IOException {

		File file = new File(fileUri);

		StringBuilder builder = new StringBuilder();

		try (FileReader reader = new FileReader(file)) {
			int data;
			while ((data = reader.read()) != -1) {
				builder.append((char) data);
			}
		}

		return builder.toString();
	}

	public static boolean exists(String fileUri) {

		File file = new File(fileUri);

		return file.exists();

	}

	public static void create(String fileUri) throws IOException {

		File file = new File(fileUri);

		file.createNewFile();

	}

	public static void delete(String fileUri) throws IOException {

		File file = new File(fileUri);

		file.delete();
	}

	public static Collection<File> getFileList(String rootFolder) {

		File folder = new File(rootFolder);

		Collection<File> files = FileUtils.listFiles(folder, null, true);

		return files;
	}

	public static void eraseContent(String fileUri) throws IOException {

		try (FileWriter fw = new FileWriter(fileUri, false)) {

			fw.write("");

			fw.flush();
		}
	}
	
	public static String getRealFolderPath(String uri, ServletContext servletContext) {
		String rootJsonFileDir = servletContext.getRealPath(uri);
		return rootJsonFileDir;
	}

}
```
### IJcJsonHelper
```

public interface IJcJsonHelper<T> {

	public String convertToJson(T bean) throws Exception;

	public T convertToBean(String jsonString, Class<T> clazz) throws Exception;

	public T readFromJsonFile(String fileUri, Class<T> clazz) throws Exception;

	public T readFromJsonFile(File file, Class<T> clazz) throws Exception;

	public void writeToJsonFile(String fileUri, T bean) throws Exception;

	public void writeToJsonFile(File file, T bean) throws Exception;
}
```
### JcJsonHelper
```
@Component("jcJsonUtil")
public class JcJsonHelper<T> implements IJcJsonHelper<T> {

	protected final Logger jcLogger = LoggerFactory.getLogger(this.getClass());

	@Override
	public String convertToJson(T bean) throws Exception {

		ObjectMapper mapper = new ObjectMapper();
		return mapper.writeValueAsString(bean);

	}

	@Override
	public T convertToBean(String jsonString, Class<T> clazz) throws Exception {

		ObjectMapper mapper = new ObjectMapper();
		return mapper.readValue(jsonString, clazz);

	}

	@Override
	public T readFromJsonFile(File file, Class<T> clazz) throws Exception {

		ObjectMapper mapper = new ObjectMapper();
		return mapper.readValue(file, clazz);

	}

	@Override
	public T readFromJsonFile(String fileUri, Class<T> clazz) throws Exception {

		return readFromJsonFile(new File(fileUri), clazz);

	}

	@Override
	public void writeToJsonFile(String fileUri, T bean) throws Exception {

		writeToJsonFile(new File(fileUri), bean);

	}

	@Override
	public void writeToJsonFile(File file, T bean) throws Exception {

		ObjectMapper mapper = new ObjectMapper();
		mapper.writeValue(file, bean);

	}
}

```
### IJcFileInfo
```
public interface IJcFileInfo<T> {
	public String getJsonFolderLocation();
	public String getUri(T bean);
}
```
## RootFolderSetting
```
@Component("rootFolderSetting")
@ConfigurationProperties(prefix = "json.crud.root.folder")
public class RootFolderSetting {

	private String location;

	public String getLocation() {
		return location;
	}

	public void setLocation(String location) {
		this.location = location;
	}
	
	
}
```
## MyFileInfo
```
@Component("myFileInfo")
public class MyFileInfo<T extends ThresholdSetting> implements IJcFileInfo<T> {

	@Autowired
	private RootFolderSetting rootFolderSetting;

	@Override
	public String getJsonFolderLocation() {
		String rootFolder = rootFolderSetting.getLocation();
		return rootFolder;
	}

	@Override
	public String getUri(T bean) {

		// logic

		return null;
	}

}

```
