# 시작하기

Rust에 기여해 주셔서 감사합니다! 기여하는 방법은 다양하며, 모든 기여에 감사드립니다.

<!-- toc -->

처음 기여하는 경우 [walkthrough] 챕터를 읽으면 전형적인 기여 과정을 예제로 볼 수 있습니다.

이 문서는 모든 것을 포괄하는 문서가 *아닙니다*. 가장 유용한 정보에 대한 빠른 가이드를 제공하기 위한 것입니다. 더 많은 정보는 [컴파일러 빌드 및 실행 방법에 관한 챕터](./building/how-to-build-and-run.md)를 참고하세요.

[internals]: https://internals.rust-lang.org
[rust-discord]: http://discord.gg/rust-lang
[rust-zulip]: https://rust-lang.zulipchat.com
[coc]: https://www.rust-lang.org/policies/code-of-conduct
[walkthrough]: ./walkthrough.md
[Getting Started]: ./getting-started.md

## 질문하기

질문이 있는 경우, [Rust Zulip 서버][rust-zulip]나 [internals.rust-lang.org][internals]에 게시글을 올려주세요. Rustup에 기여하는 경우 Zulip에는 없으며, [Discord][rust-discord]의 `#wg-rustup` 채널에서 질문할 수 있습니다.

공식 웹사이트의 [팀 및 워킹 그룹 목록][governance]과 [커뮤니티 페이지][community]에서도 더 많은 리소스를 확인할 수 있습니다.

[governance]: https://www.rust-lang.org/governance
[community]: https://www.rust-lang.org/community

모든 기여자는 [행동 강령][coc]을 따라야 한다는 점을 기억해 주세요.

컴파일러 팀(`t-compiler`)은 보통 [Zulip의 이 스트림][z]에 모여 있으며, 질문을 하기 가장 좋은 장소입니다.

[z]: https://rust-lang.zulipchat.com/#narrow/stream/131828-t-compiler

**꼭 질문해주세요!** 많은 분들이 "전문가의 시간을 낭비하는 건 아닐까" 걱정하지만, `t-compiler` 팀은 그렇게 생각하지 않습니다. 기여자는 우리에게 소중한 존재입니다.

가능하다면 공개 채널에서 질문해 주세요. 이렇게 하면 다른 사람도 질문과 답변을 볼 수 있고, 이 가이드에 반영될 수도 있습니다 :)

### 전문가 찾기

`t-compiler`의 모든 멤버가 `rustc`의 모든 부분에 전문성을 갖춘 것은 아닙니다. Rust는 상당히 큰 프로젝트입니다. 컴파일러의 각 부분에 대해 누가 전문성이 있는지 알고 싶다면, [triagebot assign 그룹][map]을 참고하세요. 이는 `triagebot.toml` 파일의 `[assign*`으로 시작하는 섹션들입니다.

하지만 누굴 태그해야 할지 모를 때도 질문하는 것을 두려워하지 마세요.

또한 최근 커밋을 살펴보는 것도 좋은 방법입니다. 예를 들어, 1.68.2 릴리즈 이후 이름 해석(name resolution) 관련 작업을 한 사람을 찾고 싶다면 다음 명령어를 실행해 보세요:

```bash
git shortlog -n 1.68.2.. compiler/rustc_resolve/
```

"Rollup merge"로 시작하는 커밋이나 `@bors`의 커밋은 무시하세요. (이러한 커밋에 대한 자세한 내용은 [CI 기여 절차](./contributing.md#ci)를 참고하세요.)

[map]: https://github.com/rust-lang/rust/blob/master/triagebot.toml

### 예절

질문할 때 가능한 많은 유용한 정보를 포함해 주세요. Rust에 처음 기여하는 경우 이게 어려울 수 있다는 것도 알고 있습니다.

아무런 맥락 없이 누군가를 태그하면 혼란을 줄 수 있으니 주의해 주세요. `t-compiler` 멤버들은 하루에도 많은 태그 알림을 받습니다.

## 무엇을 하면 좋을까요?

Rust 프로젝트는 규모가 크기 때문에 어디서부터 시작하면 좋을지, 어떤 부분이 도움이 필요한지 알기 어려울 수 있습니다. 아래에 몇 가지 추천을 소개합니다.

### 쉬운 문제 또는 멘토가 있는 이슈

시작할 곳을 찾고 있다면, [이슈 검색][help-wanted-search]을 참고하세요. [Triage]에서 이러한 라벨에 대해 설명합니다. 관심 있는 영역에 따라 검색 결과를 필터링해볼 수도 있습니다.

예를 들어:

* `repo:rust-lang/rust-clippy`는 clippy 관련 이슈만 표시합니다.
* `label:T-compiler`는 컴파일러 관련 이슈만 표시합니다.
* `label:A-diagnostics`는 진단 관련 이슈만 표시합니다.

중요하거나 초보자에게 적합한 작업이라고 해서 모두 라벨이 달려 있는 것은 아닙니다.

[help-wanted-search]: https://github.com/issues?q=is%3Aopen+is%3Aissue+org%3Arust-lang+no%3Aassignee+label%3AE-easy%2C%22good+first+issue%22%2Cgood-first-issue%2CE-medium%2CEasy%2CE-help-wanted%2CE-mentor+-label%3AS-blocked+-linked%3Apr+
[Triage]: ./contributing.md#issue-triage

### 반복적인 작업

어떤 작업은 한 사람이 하기엔 너무 큰 경우가 있습니다. 이럴 땐 여러 사람이 협업할 수 있도록 "Tracking issue"가 존재합니다. 다음은 시간 투자 없이 작업을 시작할 수 있는 추적 이슈 예시입니다:

* [UI 테스트를 하위 디렉토리로 이동](https://github.com/rust-lang/rust/issues/73494)

이외에 반복적인 작업을 발견하면 이 목록에 자유롭게 추가해주세요!

### Clippy 이슈

[Clippy] 프로젝트는 초보자 친화적인 기여 과정을 오랫동안 다듬어왔습니다. Rust 컴파일러 구조에 익숙해지기 위한 좋은 출발점입니다.

시작하려면 [Clippy 기여 가이드][clippy-contributing]를 참고하세요.

[Clippy]: https://doc.rust-lang.org/clippy/
[clippy-contributing]: https://github.com/rust-lang/rust-clippy/blob/master/CONTRIBUTING.md

### 진단 관련 이슈

진단 이슈는 대부분 독립적이며 컴파일러에 대한 배경 지식 없이도 작업할 수 있습니다. [여기에서][diagnostic-issues] 진단 이슈 목록을 확인할 수 있습니다.

[diagnostic-issues]: https://github.com/rust-lang/rust/issues?q=is%3Aissue+is%3Aopen+label%3AA-diagnostics+no%3Aassignee

### 중단된 PR 이어받기

어떤 기여자는 PR을 보내지만 시간이 부족하거나 흥미를 잃어 작업을 중단할 수 있습니다. 이러한 PR은 종종 `S-inactive` 라벨이 붙고 종료됩니다. 이런 PR을 살펴보고 이어서 작업할 수도 있습니다. [여기][abandoned-prs]에서 그런 PR을 찾을 수 있습니다.

이미 다른 방식으로 구현된 경우 `S-inactive` 라벨은 제거되어야 합니다. 그렇지 않고 여전히 해당 변경에 대한 관심이 있다면, PR을 최신 `master` 브랜치에 rebase하고 새로운 PR로 제출해 이어서 작업할 수 있습니다.

[abandoned-prs]: https://github.com/rust-lang/rust/pulls?q=is%3Apr+label%3AS-inactive+is%3Aclosed

### 테스트 작성하기

해결된 이슈 중 회귀 테스트가 없는 경우 `E-needs-test` 라벨이 붙습니다. 단위 테스트 작성은 위험도가 낮고 우선순위가 낮은 작업으로, 새로운 기여자가 테스트 인프라와 기여 과정을 익히기에 좋은 기회입니다.

### 표준 라이브러리(std)에 기여하기

[std-dev-guide](https://std-dev-guide.rust-lang.org/)를 참고하세요.

### 다른 Rust 프로젝트에 코드 기여하기

`rust-lang/rust` 외에도 `cargo`, `miri`, `rustup` 등 다양한 프로젝트에 기여할 수 있습니다.

이들 저장소는 각각 고유의 기여 가이드라인과 절차를 가질 수 있으며, 많은 경우 워킹 그룹이 관리합니다. 자세한 내용은 각 저장소의 README를 참고하세요.

### 다른 기여 방법들

Rust의 큰 코드베이스에 바로 뛰어들기 부담스럽다면, 다음과 같은 다른 기여 방법도 있습니다. 대부분은 배경지식 없이도 가능하며 매우 유용합니다:

* [문서 작성][wd]: 코드 일부를 읽고 doc 주석을 작성해보세요. 학습과 동시에 유용한 결과물을 남길 수 있습니다.
* [이슈 분류][triage]: 이슈를 분류하고 재현하며 축소하는 작업은 유지보수자에게 큰 도움이 됩니다.
* [워킹 그룹 참여][wg]: 다양한 주제의 워킹 그룹들이 있습니다.
* [Rust Discord 서버][rust-discord], [users.rust-lang.org][users], [StackOverflow][so]에서 질문에 답해보세요.
* [RFC 프로세스 참여](https://github.com/rust-lang/rfcs)
* [커뮤니티에서 요청한 라이브러리][community-library]를 찾아 구현하고 [Crates.io](http://crates.io)에 배포해보세요. 쉽진 않지만 매우 가치 있는 작업입니다.

[rust-discord]: https://discord.gg/rust-lang
[users]: https://users.rust-lang.org/
[so]: http://stackoverflow.com/questions/tagged/rust
[community-library]: https://github.com/rust-lang/rfcs/labels/A-community-library
[wd]: ./contributing.md#writing-documentation
[wg]: https://rust-lang.github.io/compiler-team/working-groups/
[triage]: ./contributing.md#issue-triage

## 클론 및 빌드하기

[컴파일러 빌드 및 실행 방법](./building/how-to-build-and-run.md) 챕터를 참고하세요.

## 기여자 절차

이 섹션은 ["기여 절차"](./contributing.md) 챕터로 이동되었습니다.

## 기타 리소스

이 섹션은 ["이 가이드에 대하여"][more-links] 챕터로 이동되었습니다.

[more-links]: ./about-this-guide.md#other-places-to-find-information
