**(이 저장소는 폐기처분 합니다.)**

# RealGrid DEMO 사이트

## Architectures

### Frameworks

- Jekyll 3.2.1
- github pages

### Libraries

- realgrid
- jquery
- bootstrap

## Development

### 저장소

- 개발 저장소(Private) : https://github.com/realgrid/demo.realgrid.com.git

### local pc에서 개발하기(배포개발자 전용)

- 배포개발자가 내 PC에서 개발할 때에는 development 환경값을 준다. `./deploy`로 배포 할때 production 환경값이 들어가기 때문.
- 이 환경값은 실제 사이트 운영시 빠져야 하는 부분들을 포함하고 있기 때문에 이 환경값없이 개발 하면 많이 불편하다.
- 환경값을 포함한 jekyll 실행 명령은 다음과 같다.
  ```
  > JEKYLL_ENV=development jekyll build
  ```

### 메뉴 카테고리 구분

- 카테고리 없는 일반 페이지
  - 카테고리 없는 일반페이지는 어디에서든 *.md 또는 *.html 등의 파일로 만들고 page를 layout으로 정해주면 된다.
  - 카테고리 없는 일반페이지는 왼쪽 메뉴의 상단에 표시되며 순서는 order 머릿말에 의해 결정된다.
- 카테고리로 구분된 demo 페이지
  - 왼쪽 메뉴 카테고리는 _config.yml 파일에 categories 속성으로 정의한다.
  - 카테고리로 구분되는 데모페이지는 _demos 폴더에, 각 카테고리명에 해당하는 하위폴더를 생성하고 그 아래에 .md 파일을 만든다.
  - 오른쪽 개발자를 위한 소스코드들어가는 영역을 devbox라 한다.
  - 이곳은 devbox라는 이름의 하위 폴더에 devboxfile 머릿말에 해당하는 파일명으로 .md파일을 만들면 자동으로 페이지에 로드된다.
  - markdown 파일에서 공백이 들어간 카테고리명을 사용하려면 ['카테고리 명'] 또는 행 구분 하여 - 카테고리 명 으로 넣어 주어야 한다.
- Release 카테고리
  - release 카테고리는 고정된 이름의 카테고리이다.
  - _release 폴더에 들어 있는 .md파일은 release로 구분 된다.

### 메뉴 필터링

- sidebar 메뉴는 page.title, page.tags로 필터링 한다.
- tags로 검색 조건을 조절 할 수 있다.

### RealGrid 버전 변경하기

- lib/realgrid폴더에 사용할 버전의 폴더를 추가한다.
- _config.yml 파일에 `realgrid_version`속성에 사용할 버전을 입력한다.
- 라이선스 파일은 /lib/realgrid폴더 루트에 위치해야 한다.
- 명령라인에 jekyll build 로 다시 빌드한다.

### demo 페이지에서 RealGrid 사용하기

- RealGrid는 lib/realgrid/realgridjs_{eval}.{version} 폴더 밑에 버전별 폴더를 만들어 넣어준다.
- 데모페이지에 로드되는 RealGrid의 버전은 _config.yml파일에 realgrid_version 속성에 지정한다.
- include 코드는 /_include/footer.html 파일에 있다.

### URL 링크

- 사용자가 접근 하는 root URL은 www.realgrid.com/demo
- 개발은 /demo 부분만 따로 관리하게 되므로 항상 앞쪽에 /demo가 붙어야 한다.
- 이러한 처리를 위해 지킬에서는 prepend 필터를 써서 아래와 같이 코딩 한다.
- markdown 파일 내에서도 사용가능.

```
$(document).ready( function(){
    RealGridJS.setRootContext("{{ "/lib/realgrid/realgridjs_eval.1.1.19" | prepend: site.baseurl }}");
    ...
Provider);
});
```

### 페이지 머릿말(frontmatter)

- 일반 page
  - layout: page
  - title: {타이틀} 상단 제목
  - description: {설명} 제목 아래 부제목 또는 페이지 설명

- demo page
  - layout: page
  - title: {타이틀} 상단 제목
  - devbox: {true|false} devbox표시여부
  - devboxfile: {filename.md} devbox에 표시될 markdown filename
  - version: 해당 페이지의 기능을 사용할 수 있는 최소 버전을 명식한다. 잘 모르면 1.0.11로 한다.
  - tags: 페이지검색용 태그
  - description: {설명} 제목 아래 부제목 또는 페이지 설명
  - categories: 데모 페이지 카테고리명

- release page
  - layout: page
  - title: {타이틀} 상단 제목
  - releaseDate: {배포일자} 정렬 순서를 결정한다.
  - sidebar: {true|false} 왼쪽 메뉴에 보일지 말지 결정한다.
  - releaseVersion: {배포버전}

### SEE

- [지킬 성능 이슈](https://wiredcraft.com/blog/make-jekyll-fast/)

## Deployment

### 배포소스

- 배포는 _site폴더의 내용을 github-pages 서비스를 위한 배포 저장소에 올린다.

### 저장소

- 배포 저장소(Public): https://github.com/RealGridSites/demo.git
- branch: 배포 브랜치는 gh-pages 이며 기본 도메인은 www.realgrid.com/demo 가 된다.

### 배포방법

- ./deploy 스크립트를 실행
- 스크립트 내부에는
  - `JEKYLL_ENV=production jekyll build` 로 환경값을 production으로 해서 _posts폴더의 내용을 메뉴에서 감춘다.
  - _site 폴더의 내용을 모두 commit 하고 push한다.

### 배포폴더 clean

- _site폴더를 지울 필요가 있을때

```js
jekyll clean
git clone https://github.com/RealGridSites/demo.git _site/
git remote rename origin deploy
```

### github pages include_releate 관련 오류

- github pages에서 발행하는 이상한 오류로 _site/.gitignore 파일에 **/devbox/ 추가로 `devbox`폴더를 강제로 제거한다.
- #1 참고
