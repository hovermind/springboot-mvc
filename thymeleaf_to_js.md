## JS variable
`/*[[ - ]]*/` syntax is used by Thymeleaf to evaluate variables used by Javascript, without breaking the script if that where to be statically loaded.
```
<script th:inline="javascript">
    function edit() {
        var link = /*[[@{/edit.html}]]*/ 'test';
        document.getElementById("user_form").action = link;
    }
</script>
```
**Details:** [Script inlining](http://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html#script-inlining-javascript-and-dart)
