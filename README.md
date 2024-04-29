<!-- markdownlint-disable-next-line -->
<div align="center">

  <!-- markdownlint-disable-next-line -->

# [S/W Tech Blog](https://onpoomswteam.github.io/)

(주) 온품 CMS본부 S/W연구개발팀 Tech Blog

[![Gem Version](https://img.shields.io/gem/v/jekyll-theme-chirpy?color=brightgreen)][gem]&nbsp;
[![CI](https://github.com/cotes2020/jekyll-theme-chirpy/actions/workflows/ci.yml/badge.svg?branch=master&event=push)][ci]
&nbsp;
[![GitHub license](https://img.shields.io/github/license/cotes2020/jekyll-theme-chirpy.svg)][license]&nbsp;

</div>

## Posting

- 중앙 저장소의 내용을 Pull 받아서 새로운 내용 확인
- \_posts 폴더 하위에 { **YYYY-MM-dd-제목.md** } 파일 생성 후 내용 작성
  - Noti :exclamation: ) **파일 내용에 공백을 포함하면 안된다.**
- image 는 assets/img 폴더 하위에 폴더를 생성하여 image 삽입 후 상대경로로 작성 <br/>
  - Example :question: ) `![](./assets/img/logos/onpoom_logo.png)`
- { **YYYY-MM-dd-제목.md** } 파일 최상단에 취소선까지 모두 포함하여 필수로 들어가야 되는 내용
  ```
  ---
  title: 게시글 제목
  author: 작성자 ID
  date: YYYY-MM-DD / 작성 날짜
  categories: [Spring] / 작성되는 순서대로 Tree 구조로 구현
  tags: [Spring, IOC, DI, Bean] / 해당되는 내용에 대한 Tag
  ---
  ```
- 제안된 Topic 을 작성하였거나, 새로운 Topic을 작성하게 되면 README.md 파일의 Topic, 필자 을 업데이트 한다.

## Topic

|                제안된 Topic                 |       필자       |        포스팅         |
|:----------------------------------------:|:--------------:|:------------------:|
|            Vue - VueQuery 란?             |                |                    |
|        Vue - Vue.js 2 vs Vue.js 3        |                |                    |
|         RabbitMQ - RabbitMQ에 대하여         | Moon Kyoung-Su | :heavy_check_mark: |
|        Java - Collection 에 대한 이해         |   Ji Min-Jae   | :heavy_check_mark: |
|             RPC - gRPC에 대하여              | Moon Kyoung-Su | :heavy_check_mark: |
| Redis - Spring 과 Redis 그리고 Message Queue | Moon Kyoung-Su | :heavy_check_mark: |
|          SDM - TDD, BDD, DDD 란?          | Kang Jeong-Woo | :heavy_check_mark: |
|    SDM - Software Design Pattern 의 종류    |   Ji Min-Jae   | :heavy_check_mark: |
|     Spring - Sl4j / Logback & Log4j2     | Lee Dae-Hyung  | :heavy_check_mark: |
|           Redis - Redis 에 대하여            | Moon Kyoung-Su | :heavy_check_mark: |
|           Java - 입출력 및 바이트스트림            | Lee Dae-Hyung  | :heavy_check_mark: |
|          Java - 8 Version 의 특장점          | Lee Dae-Hyung  | :heavy_check_mark: |
|              Jira - 실무 도입기               |   Ji Min-Jae   | :heavy_check_mark: |
|        Socket 통신의 기본 - TCP / UDP         | Kang Jeong-Woo | :heavy_check_mark: |
|           Web 기본 - WS, WA, WAS           | Kang Jeong-Woo | :heavy_check_mark: |
|     Git - Message Convention & Flow      | Moon Kyoung-Su | :heavy_check_mark: |
|          Spring - IOC, DI, Bean          | Lee Dae-Hyung  | :heavy_check_mark: |

## Word

- SDM ( Software Development Methodology ) : 소프트웨어 개발 방법론

## License

This project is published under [MIT License][license].

[gem]: https://rubygems.org/gems/jekyll-theme-chirpy

[ci]: https://github.com/cotes2020/jekyll-theme-chirpy/actions/workflows/ci.yml?query=event%3Apush+branch%3Amaster

[license]: https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/LICENSE
