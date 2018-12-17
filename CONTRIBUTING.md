# <a name="contributing"></a>참여

Xamarin 설명서 기여에 관심을 가져주셔서 감사합니다!

이 토픽에서는 [Xamarin 설명서 사이트](https://docs.microsoft.com/xamarin)에서 콘텐츠를 추가하거나 업데이트하는 기본 프로세스를 확인합니다.

이 항목에서는 다음을 다룹니다.

* [기여 절차](#process-for-contributing)
* [예제 구조](#example-structure)
* [권고 및 금지](#dos-and-donts)
* [문서 빌드](#building-the-docs)
* [샘플에 기여](#contributing-to-samples)
* [기여자 라이선스 계약](#contributor-license-agreement)

## <a name="process-for-contributing"></a>기여 절차

**1단계:** 작성하려는 문서 및 기존 콘텐츠와의 관련성을 설명하는 문제를 엽니다.
**docs** 폴더 내의 콘텐츠는 콘텐츠 영역(예: 디버거)별 분류된 섹션으로 구성됩니다. 작성한 콘텐츠가 포함된 폴더가 올바른지 확인하고, 제안 사항에 대한 피드백을 얻으십시오.

작은 변경 내용이라면 이 첫 번째 단계를 건너뛸 수 있습니다.

**2단계:** `MicrosoftDocs/xamarin-docs.ko-kr` 리포지토리를 포크합니다..

**3단계:** 작성하기 위한 문서의 `branch`를 만듭니다

**4단계:** 문서를 작성합니다.

새 항목인 경우 작성 지침과 작성자 정보 등의 각 문서에 필요한 메타데이터 설명이 되어 있는 [템플릿 파일](./styleguide/template.md)을 기반으로 만들 수 있습니다.

1단계의 문서 의해 결정된 목차 위치에 해당하는 폴더로 이동합니다.
그 폴더에는 해당 섹션의 모든 문서에 대한 Markdown 파일이 포함됩니다. 필요한 경우에 콘텐츠 파일을 배치할 새 폴더를 만듭니다.

이미지 및 다른 정적 리소스의 경우 **미디어**라는 하위 폴더에 추가합니다. 콘텐츠용 폴더를 만드는 경우 새 폴더에 미디어 폴더를 추가합니다.

적절한 Markdown 구문으로 작성해야 합니다. 자세한 내용은 [스타일 가이드](./styleguide/template.md)를 참조하세요.

### <a name="example-structure"></a>예제 구조

    docs
        /standard-library
            wstring-convert-class.md
            /media
                wstring-conversion.png

**5단계:** 분기의 PR(Pull Request, 끌어오기 요청)을 `MicrosoftDocs/xamarin-docs.ko-kr/master`에 제출합니다.

PR이 기존 문제를 해결하는 경우 `Fixes #Issue_Number` 키워드를 커밋 메시지 또는 PR 설명에 추가합니다. 그러면 PR이 병합될 때 문제가 자동으로 닫힐 수 있습니다. 자세한 내용은 [커밋 메시지를 통해 문제 닫기](https://help.github.com/articles/closing-issues-via-commit-messages/)를 참조하세요.

Visual Studio 팀은 PR을 검토하고 승인하기 위해 변경 내용이 적합하거나 다른 업데이트/변경 내용이 필요한지를 알려줄 수 있습니다.

**6단계:** Visual Studio 팀과 논의한 대로 분기에 필요한 모든 업데이트를 수행합니다.

피드백이 적용되고 변경 내용이 적합하면 유지 관리자는 PR을 마스터 분기에 병합합니다.

추후 모든 커밋을 마스터 분기에서 라이브 분기로 푸시 하면, 기여한 내용을 [docs.microsoft.com](https://docs.microsoft.com/xamarin/)에서 라이브로 확인할 수 있습니다.

## <a name="dos-and-donts"></a>권고 및 금지

다음은 설명서에 기여할 때 염두해야 하는 지침 규칙 요약입니다.

- **금지** 대규모 끌어오기 요청 행위. 대신 작업 시간을 들이기 전에 모두가 전체적인 수정 방향에 동의할 수 있도록 문제를 저장하고 토론을 시작합니다.
- **권고** [스타일 가이드](./styleguide/template.md) 및 [어투 및 어조](./styleguide/voice-tone.md) 지침을 필독하세요.
- **권고** [템플릿](./styleguide/template.md) 파일을 작업 내용의 기반으로 사용합니다.
- **권고** 문서에서 작업하기 전에 포크로 별도 분기를 만듭니다.
- **권고** [GitHub 흐름 워크플로](https://guides.github.com/introduction/flow/)를 수행합니다.
- **권고** 기여에 대해 자주 블로깅하고 페북하고 트윗, 무엇이든! 합니다.

> [!참고]
> 현재 몇몇 항목은 여기 및 [스타일 가이드](./styleguide/template.md)에 지정된 모든 지침을 따르지는 않는다는 것을 알 수 있습니다. 우리는 사이트 전체가 일관성을 유지하도록 노력하고 있습니다. 우리는 사이트 전체가 일관성을 유지하도록 노력하고 있습니다. 일관된 사이트가 되도록 현재 추적 중인 [열린 문제](https://github.com/MicrosoftDocs/xamarin-docs.ko-kr/issues?q=is%3Aissue+is%3Aopen+label%3Aguidelines-adherence)를 확인하세요.

## <a name="building-the-docs"></a>문서 빌드

이 문서는 [GitHub형 마크다운](https://help.github.com/categories/writing-on-github/)으로 작성되고 [DocFX](https://dotnet.github.io/docfx/) 및 기타 내부 게시/제작 도구를 사용하여 빌드되며 [docs.microsoft.com](https://docs.microsoft.com/)에서 호스팅됩니다.

문서를 로컬에서 빌드하려면 [DocFX](https://dotnet.github.io/docfx/)를 설치해야 합니다. 가능하면 최신 버전을 사용하세요.

DocFX를 사용하는 방법은 여러 가지가 있고 대부분 [DocFX 시작 안내서](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html)에서 다룹니다. 다음 지침은 [명령줄 기반](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool) 버전의 도구를 사용합니다. 위에 있는 링크 방법외 다른 익숙한 방법이 있다면 자유롭게 사용하세요.

**참고:** 현재 DocFX에는 Windows용 .NET Framework나 Linux 또는 macOS용 Mono가 필요합니다. 나중에 .NET Core에 이식하려고 합니다.

기본 제공 웹 서버를 사용하여 로컬에서 결과 사이트를 빌드하고 미리 볼 수 있습니다.로컬에서 `docs.microsoft.com/xamarin\docs` 폴더로 이동하고, 다음 명령을 입력합니다.

> docfx -t default --serve

그러면 [localhost:8080](http://localhost:8080)에서 로컬 미리 보기를 시작합니다. `http://localhost:8080/[path]`와 같이 내용을 확인합니다. 예를 들어 `http://localhost:8080/xamarin/xamarin.html`과 같습니다.

**참고:** 현재 로컬 미리 보기에는 테마가 포함되지 않아 모양과 느낌은 설명서 사이트와 동일하지 않습니다. 해당 환경을 수정하려고 노력하고 있습니다. 또한 미리 보기에는 문서에 삽입된 비디오, 메모나 포함된 문서 등 사용자 정의 확장을 사용한 것은 미리 보기에 표시되지 않습니다.

# <a name="contributing-to-samples"></a>샘플에 기여

지금은 필요한 샘플코드가 있다면 인라인 코드 블록으로 포함시키세요. 저장소에는 코드 조각 폴더(codesnippets)가 있지만 내부적으로만 사용하며 공개될 준비가 되지 않았습니다.

# <a name="contributor-license-agreement"></a>기여자 라이선스 계약

PR(Pull Request, 끌어오기 요청)이 병합되기 전에 [기여자 라이선스 계약(Contribution License Agreement, CLA)](LICENSE)에 서명해야 합니다. docs.microsoft.com에서는 처음에 한번만 서명하면 기록이 유지됩니다. 위키피디아에서 [기여자 라이선스 계약(CLA)](https://en.wikipedia.org/wiki/Contributor_License_Agreement)에 대한 자세한 내용을 볼 수 있습니다.

계약에 꼭 서명 할 필요는 없습니다. 일반적인 PR 제출, 복제, 포크가 가능합니다. PR이 생성되면 CLA 봇에 의해 분류됩니다. 변경이 간단한 (예 : 오타를 수정 한 경우), PR에는 CLA가 필요하지 않다고 라벨링을 합니다. 이런경우가 아니라면 CLA 필수로 분류됩니다. CLA에 서명하면 현재 및 향후 모든 끌어 오기 요청(Pull Request, PR)에 CLA 서명이 표시됩니다.
