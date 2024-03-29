---
title: Git - Message Convention & Flow
author: kyoungsu0717
date: 2024-03-22
categories: [ Git ]
tags: [ Git, Flow, SCM ]
image:
  path: /assets/img/20240322/thumbnail.jpg
  alt: Git.
---

회사에서 프로젝트를 진행하다 보면 한 개의 프로젝트를 혼자 진행하는 경우도 있지만, 협업이 주를 이룬다. <br>
일정에 따라서 프로젝트를 부분부분 진행하기도 하고 바꿔 진행하기도 하면서 프로젝트를 완성시키는 경우가 대부분이다.<br>
그러므로 프로젝트의 흐름 및 변경점에 대한 History의 빠른 파악에 필요한 전략인 Git Message Convention 과 협업을 용이하게 하기 위한 Git Flow 전략을 정리해보려고 한다.

# 🚩 Git Message Convention

## 📝 Git Message Convention 이란 ?

### ■ Git Message Convention 정의

> 프로젝트의 흐름 및 변경점을 빠르게 파악하기 위하여 Commit Message 를 Udacity Style Guide 에 맞춰서 작성하는 전략
{: .prompt-info }

### ■ Udacity Style Guide 란?

> 미국의 영리 교육 기관으로 온라인 강의를 제공하는 기관인 Udacity 가 Git Commit Message 의 구조를 Subject ( 제목 ), Body ( 내용 ), Footer ( 참조 ) 로
> 정의하고 <br>
> 구조에 들어가는 내용들을 어떻게 작성하는지 설명해 놓은 것이 Udacity Style Guide 이다.
{: .prompt-info }

## 📝 Git Message Convention 구조

### ○ Subject - 제목 **( 필수 )**

* **작성법**
  1. 길이는 50자 이하로 작성한다.
  2. 동사원형( ex. Feat, Fix, Docs, Comment )로 시작한다.
  3. 첫 글자는 대문자이다.
  4. 제목의 끝에는 마침표를 붙이지 않는다.


* **동사원형**

  |    이름    |                 설명                  |
  |:--------:|:-----------------------------------:|
  |   Feat   |             새로운 기능을 추가              |
  |   Fix    |                버그 수정                |
  |  Design  |     CSS & SCSS 등 사용자 UI 디자인 변경      |
  |  Hotfix  |           급하게 치명적인 버그를 수정           |
  |  Style   |   코드 포맷 변경, 세미콜론 누락, 코드 수정이 없는 경우   |
  | Refactor |            프로덕션 코드 리팩토링             |
  | Comment  |           필요한 주석 추가 및 변경            |
  |   Docs   |                문서 수정                |
  |   Test   |    테스트 추가, 테스트 리팩토링 ( 기능 변경 X )     |
  |  Chore   | 빌드 테스트 업데이트, 패키지 매니저 설정 ( 기능 변경 X ) |
  |  Rename  |          파일 혹은 폴더명 수정 & 이동          |
  |  Remove  |                파일 삭제                |

### ○ Body - 내용  **( 필수 )**

* **작성법**
  1. 한 문장 당 길이는 72자 이하로 작성한다.
  2. **( CLI 명령어 실행 시 )** Subject 과 Body 사이에는 한 줄의 공백을 추가한다.
  3. Commit Message 의 What 과 Why 에 대해 작성하며 How 에 대한 사항을 작성하지 않는다.

### ○ Footer - 참조 **( 선택사항 )**

* **작성법**
  1. 문제가 된 Commit 의 ID를 작성한다.
  2. 해결한 Issue 의 ID를 작성한다.
  3. 문제가 해결 하기 위해 참고한 Commit 의 ID를 작성한다.
  4. 관련된 팀원들의 ID를 작성한다.

## 📝 Github Desktop - Git Message Convention 예시 작성법

#### ○ 기능이 추가된 경우 예시

< Subject > : 제목 <br>

```
Feat: 기능 추가
```

< Body > : 내용 <br>

```
- 기능 1 추가
- 기능 2 추가
- 기능 3 추가
```

##### Github Desktop 적용 이미지

![](assets/img/20240322/기능추가01.jpg)

#### ○ 문서만 변경이 된 경우

< Subject > - 제목 <br>

```
Docs: 문서 수정
```

< Body > - 내용 <br>

```
- README.md 파일 수정
- application.yml 파일 수정
- log4j2.xml 파일 수정
```

##### < Github Desktop 적용 이미지 >

![](assets/img/20240322/문서수정01.jpg)

# 🚩 Git Flow

## 📝 Git Flow 이란 ?

### ■ Git Flow 정의

> 직역 하여 흐름이라는 의미로서 Git 에서 제공하는 강력한 브랜칭 기능을 활용한 소스 변경 이력 관리 전략

## 📝 Git Flow 활용

### ■ Git Flow 의 대표 사진

Git Flow 전략을 적극적으로 사용 중인 '배달의 민족' 어플리케이션을 개발한 회사 '우아한 형제들' 회사의 Git Flow 활용 예시다.

![](assets/img/20240322/GitFlow-Ex01.jpg)

#### ○ main branch ( = master branch )

* 제품으로 출시될 수 있는 branch
* main branch 의 최신 버전은 언제나 실행이 가능한 프로그램이다.

#### ○ develop branch

* 현재 개발하고 있는 branch
* 단독 프로젝트라면 이 브랜치에 직접적으로 commit 하여도 상관없지만 협업 중이라면 feature 브랜치를 활용하여야 한다.
* Release 하게되면 release branch 로 Merge 한다.
* 프로젝트를 배포하게 된다면 main 브랜치로 merge 한다.

#### ○ feature branches

* 기능별 개발이 이루어지는 branch
* 다른 branch 들과는 다르게 feature branch 는 N개 존재할 수 있다.
* 프로젝트 협업 시 feature_기능명 으로 분기시켜서 다중 작업이 가능하도록 한다.
* 각자 맡은 부분이 개발이 완료되면 자신이 개발한 feature_기능명 branch 를 develop 브랜치로 merge 한다.

#### ○ hotfixes branch

* 급한 버그 수정이 이루어지는 branch
* Release 직전에 버그가 발생되는 경우 분기되는 branch 이다.
* 배포 후 현장에서 갑자기 치명적인 버그가 발생하는 분기되는 branch 이다.
* 수정이 완료되면 분기를 시작한 branch 로 merge 한다.

#### ○ release branch

* 프로젝트의 배포는 아니지만 개발이 완료되었을 경우 사용하는 branch
* 개발이 완료되어 실행이 가능한 프로그램이다.

### ■ Git Flow 의 활용 예시

현재 우리 회사에서 간략하게 사용하고 있는 Git Flow 활용 예시이다.

![](assets/img/20240322/GitFlow-Ex02.jpg)

# 🚩 마치며..

Git - Message Convention & Flow 에 대해서 정리해보았다. <br>
해당 전략들은 반드시 지켜야 한다까지는 아니지만 지키면 협업 중인 팀원, 인계 하는 팀원, 인수하는 팀원 모두들에게 너무나도 좋은 전략이다. <br>
습관이 들기전까지는 당연히 힘들겠지만 나 혼자 개발하는 것이 아니며 팀원을 생각하면 반드시라는 생각이 나도 모르게 드는 전략들이다. 
