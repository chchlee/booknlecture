# Spring의 Servlet 실행 구조

## 1. HTTP 요청

클라이언트에서 HTTP 요청이 발생하면 먼저 웹 서버(Tomcat, Jetty 등)가 이를 받아 Servlet Container로 전달합니다.

## 2. Servlet Container

Servlet Container(또는 서블릿 엔진, 웹 컨테이너)는 웹 서버 내에서 동작하며 서블릿의 생명주기를 관리하고 클라이언트의 요청을 처리하는 기능을 담당합니다.

Servlet Container의 주요 기능들은 다음과 같습니다:

- **서블릿 생명주기 관리**: Servlet Container는 서블릿의 전체 생명주기 (생성, 초기화, 요청 처리, 제거)를 관리합니다.

- **멀티스레딩 지원**: 각 요청마다 별도의 자바 스레드를 생성하여 처리하며, 이를 통해 동시에 여러 요청을 처리할 수 있습니다.

- **선언적인 보안 관리**: 웹 애플리케이션의 보안을 관리하기 위해 사용자 인증, 권한 부여 등의 기능을 제공합니다.

- **JSP 지원**: Servlet Container는 JSP 페이지를 처리하고, 이를 서블릿 코드로 변환하는 기능도 제공합니다.

Tomcat, Jetty, GlassFish, WildFly 등이 대표적인 Servlet Container입니다.

## DispatcherServlet

Spring MVC에서 DispatcherServlet은 Front Controller 디자인 패턴의 주요 구성 요소입니다. 이는 모든 웹 요청을 처리하고 각 요청을 적절한 처리자에게 전달하는 중앙 컨트롤러 역할을 합니다.

DispatcherServlet의 주요 기능은 다음과 같습니다:

- **요청 라우팅**: HTTP 요청이 도착하면 DispatcherServlet은 요청 URL을 기반으로 요청을 처리할 적절한 Controller를 찾습니다. 이때 사용되는 것이 HandlerMapping 구성 요소입니다.

- **요청 처리**: DispatcherServlet은 선택된 Controller의 메소드를 호출하여 요청을 처리하도록 합니다.

- **뷰 렌더링**: Controller 메소드에서 반환된 Model 데이터와 View 이름을 사용하여 응답을 생성합니다. 이때 사용되는 것이 ViewResolver와 View 구성 요소입니다.

- **예외 처리**: 요청 처리 과정에서 발생하는 예외를 적절하게 처리하기 위한 기능을 제공합니다.

## 3. Handler Mapping

Spring MVC에서 `HandlerMapping`은 클라이언트의 HTTP 요청을 실제로 처리할 `Handler`를 결정하는 인터페이스입니다. 즉, `HandlerMapping`은 요청 URL, HTTP 메소드, 요청 파라미터 등을 기반으로 적절한 `Controller`와 그 `Controller`의 메소드를 결정하는 역할을 합니다.

Spring MVC는 다양한 유형의 `HandlerMapping` 전략을 제공합니다:

- **BeanNameUrlHandlerMapping**: 이 전략은 요청 URL과 동일한 이름의 빈을 찾습니다. 예를 들어, '/myController'라는 요청이 들어오면, 'myController'라는 이름의 빈을 찾아서 처리하게 됩니다.

- **ControllerClassNameHandlerMapping**: 이 전략은 요청 URL과 `Controller` 클래스의 이름을 매핑합니다. 클래스 이름은 URL의 일부가 됩니다.

- **RequestMappingHandlerMapping**: 이 전략은 `@RequestMapping` 애노테이션이 붙은 메소드와 요청을 매핑합니다. 이는 Spring MVC 3.1 버전부터 도입되었으며, 대부분의 실제 애플리케이션에서 가장 일반적으로 사용하는 `HandlerMapping` 전략입니다.

- **SimpleUrlHandlerMapping**: 이 전략은 프로퍼티 파일이나 XML 설정을 사용하여 요청 URL과 `Controller`를 직접 매핑합니다.

요청이 들어오면 `DispatcherServlet`은 `HandlerMapping` 전략을 사용하여 적절한 `Controller`를 찾고, 해당 `Controller`의 메소드를 호출하여 요청을 처리하게 됩니다.

## 4. Controller

Spring MVC에서 `Controller`는 클라이언트의 요청을 처리하고 그 결과를 보여주는 뷰를 선택하는 역할을 담당합니다. `Controller`는 웹 애플리케이션의 비즈니스 로직을 처리하고 데이터를 `Model` 객체에 바인딩하여 `View`로 전달하는 중요한 역할을 합니다.

`Controller`의 주요 기능과 특징은 다음과 같습니다:

- **요청 처리**: `Controller`는 클라이언트의 HTTP 요청을 처리하고 비즈니스 로직을 실행합니다. 요청 URL, HTTP 메소드, 요청 파라미터 등에 따라 실행할 메소드를 결정합니다.

- **Model 데이터 바인딩**: `Controller`는 비즈니스 로직의 실행 결과를 `Model` 객체에 바인딩합니다. `Model` 객체는 데이터와 그 데이터를 보여주는 데 필요한 정보를 포함하고 있습니다.

- **View 선택**: `Controller`는 클라이언트에게 보여줄 `View`를 선택합니다. 이는 보통 문자열로 반환되며, 이 문자열은 `ViewResolver`에 의해 실제 `View` 객체로 변환됩니다.

## 5. View Resolver

Spring MVC에서 `ViewResolver`는 `Controller`가 반환하는 논리적인 View 이름을 실제 View 객체로 해석하는 인터페이스입니다.

`ViewResolver`의 주요 역할과 특징은 다음과 같습니다:

- **논리적 View 이름 해석**: `Controller`는 처리 결과를 표현할 View의 이름을 문자열로 반환합니다. `ViewResolver`는 이 논리적인 이름을 실제 View 객체로 변환합니다.

- **View 기술 선택**: `ViewResolver`는 특정 View 기술 (예: JSP, Thymeleaf, FreeMarker 등)에 대한 View 객체를 반환할 수 있습니다.

- **View 생성**: 실제 View 객체를 생성하고, 필요한 설정을 적용합니다.

Spring MVC는 다양한 유형의 `ViewResolver`를 제공합니다:

- **InternalResourceViewResolver**: 가장 일반적으로 사용되는 `ViewResolver`로, JSP와 같은 웹 애플리케이션 리소스를 처리합니다.

- **BeanNameViewResolver**: View의 이름을 Spring 빈의 이름으로 사용하고 해당 빈을 View로 사용합니다.

- **ThymeleafViewResolver**, **FreeMarkerViewResolver**, 등: 각각 Thymeleaf, FreeMarker 등의 템플릿 엔진에 대한 `ViewResolver`입니다.

`ViewResolver`는 `DispatcherServlet`의 설정에서 정의되며, 하나의 애플리케이션에서 여러 `ViewResolver`를 체인으로 연결하여 사용할 수 있습니다.

## 6. View

View (예: JSP 파일)는 Model 데이터를 사용하여 렌더링되며 이 결과는 HTTP 응답의 본문으로 전송됩니다.

## 7. HTTP 응답

HTTP 응답(Response)은 HTTP 요청(Request)에 대한 서버의 응답입니다. HTTP 응답은 기본적으로 상태 코드(Status Code), 헤더(Headers), 및 본문(Body)의 세 부분으로 구성됩니다.

- **상태 코드(Status Code)**: 이는 서버가 요청을 처리한 결과를 나타내는 3자리 숫자입니다. 예를 들어, 200은 성공을, 404는 리소스를 찾을 수 없음을, 500은 서버 내부 오류를 나타냅니다.

- **헤더(Headers)**: 이는 응답에 대한 메타데이터를 포함합니다. 예를 들어, 콘텐츠의 유형(`Content-Type`), 콘텐츠의 길이(`Content-Length`), 캐싱 동작(`Cache-Control`) 등이 있습니다.

- **본문(Body)**: 이는 실제 응답 데이터를 포함합니다. 이는 HTML 문서일 수도 있고, JSON 데이터일 수도 있고, 이미지나 다른 유형의 파일일 수도 있습니다.

Spring MVC에서는 `Controller`의 메소드가 HTTP 응답을 생성합니다. `Controller` 메소드는 데이터와 뷰 이름을 포함하는 `ModelAndView` 객체를 반환하거나, HTTP 상태 코드와 본문을 포함하는 `ResponseEntity` 객체를 반환할 수 있습니다.

또한 `View` 객체는 `Model` 데이터를 사용하여 HTTP 응답 본문을 생성합니다. 이는 JSP, Thymeleaf, FreeMarker 등의 템플릿 엔진을 사용하여 HTML을 생성하거나, JSON 변환 라이브러리를 사용하여 JSON을 생성할 수 있습니다.

## 서블릿의 역할

```java
@WebServlet(name = "helloServlet", urlPatterns = "/hello/")
public class HelloServlet extends HttpServlet {

  @Override
  protected void service(HttpServletRequest request, HttpServletResponse reponse) {
    // 애플리케이션 로직
  }
}
```

- urlPattern의 URL 호출되면 서블릿 코드 실행
- 요청, 응답을 편리하게 사용, 제공
- HTTP 스펙을 편리하게 사용

## 서블릿 컨테이너

- 톰캣처럼 서블릿을 지원하는 WAS를 서블릿 컨테이너
- 서블릿 컨테이너는 서블릿 객체를 생성, 초기화, 호출, 종료하는 생명주기 관리
- 서블릿 객체는 싱글톤으로 관리
  - 최초 로딩 시점에 서블릿 객체를 미리 만들어 놓고 재활용
  - 모든 고객 요청은 동일한 서블릿 객체 인스턴스에 접근
  - 서블릿 컨테이너 종료시 함께 종료
- JSP도 서블릿으로 변환되어 사용
- 동시 요청을 위한 멀티 쓰레드 처리 지원

## HTML 페이지

- 동적으로 필요한 HTML 파일을 생성해서 전달
- 웹 브라우저 : HTML 해석

## HTTP API

- 다양한 시스템에서 호출
- 데이터만 주고 받음, UI 화면이 필요하면, 클라이언트가 별도 처리
- 앱, 웹 클라이언트, 서버 to 서버

## SSR - 서버 사이드 렌더링

- 서버에서 최종 HTML을 생성해서 클라이언트에 전달
- 브라우저 서버 - html, db
- 주로 정적인 화면에 사용

## CSR - 클라이언트 사이드 렌더링

- HTML 결과를 자바스크립트를 사용해 웹 브라우저에서 동적으로 생성해서 적용
- 주로 동적인 화면에 사용, 웹 환경을 마치 앱 처럼 필요한 부분부분 변경

## GET - 쿼리 파라미터

- /url?username=hello&age=20
- 메시지 바디 없이, URL의 쿼리 파라미터에 데이터를 포함해서 전달 예) 검색, 필터, 페이징등에서 많이 사용하는 방식

## POST - HTML Form

- content-type: application/x-www-form-urlencoded
- 메시지 바디에 쿼리 파리미터 형식으로 전달 username=hello&age=20 예) 회원 가입, 상품 주문, HTML Form 사용

## HTTP message body에 데이터를 직접 담아서 요청

- HTTP API에서 주로 사용, JSON, XML, TEXT
- 데이터 형식은 주로 JSON 사용
- POST, PUT, PATCH

## 스프링MVC는 @ModelAttribute 가 있으면 다음을 실행한다

- HelloData 객체를 생성한다.
- 요청 파라미터의 이름으로 HelloData 객체의 프로퍼티를 찾는다.
- 그리고 해당 프로퍼티의 setter를 호출해서 파라미터의 값을 입력(바인딩) 한다.
- 예) 파라미터 이름이 username 이면 setUsername() 메서드를 찾아서 호출하면서 값을 입력한다.
