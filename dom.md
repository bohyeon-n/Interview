# DOM?

> "The W3C Document Object Model (DOM) is a platform and language-neutral interface that allows programs and scripts to dynamically access and update the content, structure, and style of a document."

DOM(Document Object Model)은 HTML 및 XML 문서를 노드로 구성된 트리 구조로 처리하는 cross-plaform API 이다.

여기서 노드는 간단히 말해서 HTML element 이다. DOM 은 웹사이트의 HTML 을 트리구조로 나타낸다. 모든 HTML element 는 node 이다.

노드는 조작할 수 있는 객체이며 변경된 내용이 document 에 반영된다. 브라우저에서 이 API 는 자바스크립트에서 DOM 노드를 조작하여 스타일,내용, document 의 배치를 변경하거나 이벤트 리스너를 통해 상호작용하도록 조작할 수 있다.

DOM 은 어떤 특정 프로그래밍 언어와 독립적으로 설계되어 document 의 구조적 표현을 일관성 있는 단일 API 에서 사용할 수 있다.

W3C DOM 표준은 세가지 파트로 구분된다.

- Core DOM - 모든 문서 타입의 표준 모델
- XML DOM - XML 문서의 표준 모델
- HTML DOM - HTML 문서의 표준 모델

페이지가 로드될 때 브라우저는 페이지의 Document Object Model 을 생성한다. HTML DOM 은 객체의 트리들로 구성된다. DOM 이 생성 된 후에 스크립트가 DOM 을 조작하기 때문에 에러를 피하기 위해 스크립트를 페이지의 아래쪽에 위치시켜야 한다.

`document.getElementById()`와 `document.querySelector()`는 DOM 노드를 선택하는 데 사용되는 메소드이다.

`innerHTML`속성을 새 값으로 설정하면 HTML parser 를 통해 실행되므로, 동적으로 HTML 컨텐츠를 노드에 동적으로 삽입할 수 있다.
`
