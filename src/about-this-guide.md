# 이 가이드에 대하여

이 가이드는 rustc, 즉 Rust 컴파일러가 어떻게 작동하는지를 문서화하고,
새로운 기여자가 rustc 개발에 참여할 수 있도록 돕기 위해 만들어졌습니다.

이 가이드는 여러 부분으로 구성되어 있습니다:

1. [rustc 빌드 및 디버깅][p1]:
   빌드, 디버깅, 프로파일링 등에 대한 전반적인 정보가 포함되어 있으며,
   어떤 방식으로 기여하든 유용합니다.
2. [Rust에 기여하기][p2]:
   기여 절차, git 및 GitHub 사용법, 기능 안정화 절차 등
   어떤 기여 방식이든 도움이 되는 정보가 포함되어 있습니다.
3. [부트스트래핑][p3]:
   Rust 컴파일러가 이전 버전을 이용해 스스로를 빌드하는 과정을 설명합니다.
   부트스트랩 프로세스와 디버깅 방법이 포함됩니다.
4. [고수준 컴파일러 아키텍처][p4]:
   컴파일러의 고수준 구조와 컴파일 과정의 단계들을 다룹니다.
5. [소스 코드 표현][p5]:
   사용자로부터 받은 원시 소스 코드를 컴파일러가 쉽게 다룰 수 있도록
   다양한 형태로 변환하는 과정을 설명합니다.
6. [지원 인프라][p6]:
   커맨드라인 인자 규칙, rustc\_driver나 rustc\_interface와 같은 진입점,
   오류 및 린트 설계 및 구현을 포함합니다.
7. [분석][p7]:
   타입 검사 등의 컴파일 후반부 단계를 위해 코드의 다양한 특성을
   분석하는 과정을 설명합니다.
8. [MIR부터 바이너리까지][p8]:
   MIR에서 연결된 실행 가능 기계 코드가 어떻게 생성되는지를 설명합니다.
9. [부록][p9]:
   참고용 정보를 담고 있으며, 용어집 등을 포함한 다양한 내용이 있습니다.

[p1]: ./building/how-to-build-and-run.html
[p2]: ./contributing.md
[p3]: ./building/bootstrapping/intro.md
[p4]: ./part-2-intro.md
[p5]: ./part-3-intro.md
[p6]: ./cli.md
[p7]: ./part-4-intro.md
[p8]: ./part-5-intro.md
[p9]: ./appendix/background.md

### 지속적인 변화

`rustc`는 실제로 운영 중인 고품질 제품이며,
상당수의 기여자들에 의해 지속적으로 작업되고 있습니다.

따라서 코드베이스가 자주 변경되고 기술적 부채도 존재합니다.
또한 이 가이드에서 논의되는 많은 아이디어는 이상적인 설계에 기반하고 있으며
아직 완전히 실현되지 않은 것도 많습니다.

이로 인해 가이드를 항상 최신 상태로 유지하는 것은 매우 어렵습니다!

이 가이드 자체도 오픈소스로 제공되며,
소스는 [GitHub 저장소]에서 확인할 수 있습니다.

가이드에서 오류를 발견하면 이슈를 제기해 주세요.
더 좋은 방법은 PR을 통해 수정해주는 것입니다!

문서 작성에 기여하고자 한다면,
이 가이드의 [문서 작성 관련 하위 섹션]을 참고해 주세요.

[문서 작성 관련 하위 섹션]: contributing.md#contributing-to-rustc-dev-guide

> “모든 형성된 것은 무상하다” —
> 이것을 지혜로 보면, 고통에서 벗어난다.
> *법구경 277구절*

## 다른 정보 출처

다음 사이트들도 유용할 수 있습니다:

* 이 가이드는 컴파일러 각 부분의 작동 방식과 기여 방법에 대한 정보를 담고 있습니다.
* [rustc API 문서] — 컴파일러, 개발 도구, 내부 도구들에 대한 rustdoc 문서
* [Forge] — Rust 인프라, 팀 절차 등 다양한 문서를 포함
* [compiler-team] — Rust 컴파일러 팀의 본거지로, 절차, 워킹 그룹, 팀 캘린더 등의 정보를 담고 있음
* [std-dev-guide] — 표준 라이브러리 개발을 위한 유사한 가이드
* [t-compiler Zulip 스트림][z]
* [Discord](https://discord.gg/rust-lang)의 `#contribute` 및 `#wg-rustup`
* [Rust Internals 포럼][rif] — 질문하고 Rust 내부를 논의할 수 있는 공간
* [Rust 레퍼런스][rr] — 내부 구조는 다루지 않지만 훌륭한 참고 자료
* [Tom Lee의 블로그 글][tlgba] — 지금은 구버전이지만 여전히 유익함
* [Rust 컴파일러 테스트 문서][rctd]
* [@bors] 관련해서는 [치트시트][cheatsheet]가 유용함
* Google도 항상 프로그래밍에 유용합니다.
  [Rust 공식 문서 전체를 검색][gsearchdocs]할 수 있습니다
  (표준 라이브러리, 컴파일러, 서적, 참조, 가이드 포함)
* Rustdoc의 검색 기능을 통해 타입과 함수 문서를 찾을 수 있습니다.
  타입 시그니처로도 검색이 가능합니다!
  예: `* -> vec`을 검색하면 `Vec<T>`를 반환하는 함수를 찾을 수 있습니다.
  *힌트:* Rustdoc 페이지에서 `?`를 눌러 더 많은 팁과 단축키를 확인해보세요.

[rustc dev guide]: about-this-guide.md
[gsearchdocs]: https://www.google.com/search?q=site:doc.rust-lang.org+your+query+here
[stddocs]: https://doc.rust-lang.org/std
[rif]: http://internals.rust-lang.org
[rr]: https://doc.rust-lang.org/book/
[rustforge]: https://forge.rust-lang.org/
[tlgba]: https://tomlee.co/2014/04/a-more-detailed-tour-of-the-rust-compiler/
[ro]: https://www.rustaceans.org/
[rctd]: tests/intro.md
[cheatsheet]: https://bors.rust-lang.org/
[Miri]: https://github.com/rust-lang/miri
[@bors]: https://github.com/bors
[GitHub 저장소]: https://github.com/rust-lang/rustc-dev-guide/
[rustc API 문서]: https://doc.rust-lang.org/nightly/nightly-rustc/rustc_middle
[Forge]: https://forge.rust-lang.org/
[compiler-team]: https://github.com/rust-lang/compiler-team/
[std-dev-guide]: https://std-dev-guide.rust-lang.org/
[z]: https://rust-lang.zulipchat.com/#narrow/stream/131828-t-compiler
