1. You need xmlns:th="http://www.thymeleaf.org" for it to work in spring boot.
2. th:value -> to set value of input
3. th:text -> to set inner html of an element
5. th:href -> to set link tag css file
6. th:src -> to set img, script file

Examples:
```html
<link th:href="@{/metronic/assets/plugins/global/plugins.bundle.css}" rel="stylesheet" type="text/css" />

<script th:src="@{/metronic/assets/plugins/global/plugins.bundle.js}"></script>

<img th:src="@{/images/drpak.png}" />

<div th:text="${dashboardInfo.numberOfPeopleAwaiting} + 'people'">20 people</div>
<!-- Will replace 20 people with w/e came from controller-->
```

When defining fragments, note that its file should contain html and body tag. in body you then can define your fragment
```html
<!DOCTYPE html>  
<html xmlns:th="http://www.thymeleaf.org">  
<body>  
	<th:block th:fragment="turn">   
		..........
	</th:block>  
</body>
```

```html
<!DOCTYPE html>  
<html xmlns:th="http://www.thymeleaf.org">  
<body>
<div th:fragment="_footer" id="kt_app_footer" class="app-footer
	.........
</div>
</body>
```

Using fragments in other html:
```html
<section th:replace="doctor/layout/partials/_footer.html :: _footer"></section>
```

Using fragments from controller:
```java
	@GetMapping("/dashboard")  
		public String dashboard(Model model) {  
		loadDashboardInfo(model);  
		return "/doctor/dashboard :: dashboard";
		// first template then :: fragment-name  
	}
```