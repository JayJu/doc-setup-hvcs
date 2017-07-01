# 크롬 확장프로그램

1. 기초
   * 크롬 확장프로그램은 크롬 브라우저에 특정 기능을 추가하기 위해 만든 HTML, CSS, Javascript, 이미지등의 파일들의 묶음.
   * 기본적으로 웹 페이지로 구성하며 브라우저에서 지원하는 웹 페이지용 API는 모두 사용 가능
   * content scripts나 cross-origin XMLHttpRequests 등을 사용 하여 서버와 통신 가능
   * UI
     * 크롬 앱을 포함한 확장프로그램들의 UI는 browser action 또는 page action의 형태로 구성된다
       * browser action
         * 대부분의 페이지와 연관이 있을 경우 예\) Google Mail Checker
       * page action
         * 특정 페이지에 따라 활성화/비활성화 해야 할 경우 예\) 
2. 구성파일
   1. 다음 4개의 파일들로 구성되며 하나의 폴더에 저장 하여 zip으로 압축 후 .crx 로 확장자를 변경하여 배포한다. 
      로컬의 경우 폴더째로 등록 가능

      * manifest 파일 \(반드시 하나\)
      * ```
        {
          "name": "My Extension",
          "version": "2.1",
          "description": "Gets information from Google.",
          "icons": { "128": "icon_128.png" },
          "background": {
            "persistent": false,
            "scripts": ["bg.js"]
          },
          "permissions": ["http://*.google.com/", "https://*.google.com/"],
          "browser_action": {
            "default_title": "",
            "default_icon": "icon_19.png",
            "default_popup": "popup.html"
          }
        }
        ```
      * html 파일 \(하나 이상\)
      * javascript \(옵션\)
      * 이미지 등의 resource 파일 \(옵션\)
3. 아키텍쳐
   1. 백그라운드페이지
   2. UI페이지
4. 크롬 API





