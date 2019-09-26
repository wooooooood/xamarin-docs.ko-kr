---
title: 이 문서 제목
description: 이 문서에 대한 유용하고 확실한 설명입니다. 여기에 있는 콘텐츠는 Microsoft Docs 시스템에서 검색 결과와 함께 반환되므로 이 문서 파일의 목적 및 콘텐츠에 대한 완벽한 설명이 될 수 있습니다.
keywords: 키워드, 목록, 대상, 내부, 검색
author: conceptdev
ms.author: crdun
ms.date: 02/26/2018
ms.topic: conceptual
ms.assetid: 11111111-2222-3333-4444-555555555555
ms.prod: xamarin
ms.openlocfilehash: d6300d6d3fdebf851c3710aa2a949575e6fe3f68
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71249562"
---
# <a name="metadata-and-markdown-template"></a>메타데이터 및 Markdown 템플릿

이 템플릿에는 Markdown 구문의 예제와 메타데이터 설정 지침이 포함되어 있습니다. 이 템플릿을 최대한 활용하려면 **원시 Markdown** 및 **렌더링된 보기**를 모두 확인해야 합니다. 예를 들어 원시 Markdown에는 메타데이터 블록이 표시되는 반면 렌더링된 보기에는 표시되지 않습니다.

Markdown 파일을 만들 때는 이 템플릿을 새 파일로 복사하고, 아래에 지정되어 있는 것처럼 메타데이터를 입력하고, 위의 H1 제목을 문서 제목으로 설정한 다음 내용을 삭제해야 합니다.

## <a name="metadata"></a>메타데이터

많은 주석이 있는 전체 메타데이터 블록은 다음과 같이 표시됩니다.

```
---
title: This Article's Title
description: A good, solid description of this article. The content kept here is what the Microsoft Docs system returns with search results, so this had better be a complete description of the purpose and content for this document file
keywords: a, comma, separated, list, of keywords, to, use, for, this, doc, however, this, might, not, be, used, for, anything, much, anymore, but, still, good, to, have, if, you, want
author: conceptdev (your Github user name)
ms.author: crdun (your Microsoft Alias)
ms.date: 02/26/2018 (creation date, in US format: mm/dd/yyyy)
ms.topic: conceptual (one only of the following: conceptual, quickstart, tutorial, overview)
ms.assetid: b39854a6-c523-4a66-bef6-9b5da03bafff (a unique number representing the asset - just use a GUID, you can generate one at https://www.guidgenerator.com/)
ms.prod: xamarin (no reason to change this)
ms.custom: Analytics data, a field that gets imported into SkyEye so you can use it in custom reports
---
```

몇 가지 주요 참고 사항은 다음과 같습니다.

- 메타데이터 요소의 값과 콜론(:) 사이에는 공백이 **있어야 합니다**.
- 선택적 메타데이터 요소에 값이 없는 경우 해당 요소를 삭제합니다(비워 두거나 “na” 사용).
- 제목 등의 값에 콜론이 포함되어 있으면 메타데이터 파서가 중단됩니다. 이 경우 `title: "Writing .NET Core console apps: An advanced step-by-step guide"`와 같이 큰따옴표를 사용하여 제목을 묶습니다.
- **title**: 이 제목은 검색 엔진 결과에 표시됩니다. 제목은 H1 제목의 제목과 같을 필요는 없으며 60자 이내로 작성해야 합니다.
- **author**, **manager**, **ms.reviewer**: author 필드에는 작성자의 별칭이 아닌 **GitHub 사용자 이름**을 포함해야 합니다.  반면 "manager" 및 "ms.reviewer" 필드에는 Microsoft 별칭을 포함해야 합니다. ms.reviewer는 문서나 기능과 연관된 PM/개발자의 이름을 지정합니다.
- **ms.assetid**: BI(비즈니스 인텔리전스)와 같이 내부 추적용으로 사용되는 문서의 GUID입니다. 새 Markdown 파일을 만들 때는 [https://www.guidgenerator.com](https://www.guidgenerator.com)에서 GUID를 가져옵니다.

## <a name="basic-markdown-gfm-and-special-characters"></a>기본 Markdown, GFM 및 특수 문자

모든 기본 및 GFM(GitHub Flavored Markdown)이 지원되는 것은 아닙니다. 여기에 대한 자세한 내용은 다음 항목을 참조하세요.

- [기준 Markdown 구문](https://daringfireball.net/projects/markdown/syntax)
- [DocFX Flavored Markdown](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html)
- [GFM 설명서](https://guides.github.com/features/mastering-markdown)

Markdown은 서식 지정을 위해 \*, \`, \# 등의 특수 문자를 사용합니다. 이러한 문자 중 하나를 내용에 포함하려는 경우에는 다음 두 작업 중 하나를 수행해야 합니다.

- 특수 문자 앞에 백슬래시를 넣어 특수 문자를 "이스케이프"합니다(예: \*의 경우 `\*`)
- 문자의 [HTML 엔터티 코드](http://www.ascii.cl/htmlcodes.htm)를 사용합니다(예: &#42;의 경우 `&#42;`).

## <a name="file-name"></a>파일 이름

파일 이름은 다음 규칙을 사용합니다.

- 소문자, 숫자 및 하이픈만 포함할 수 있습니다.
- 공백 또는 문장 부호 문자는 포함할 수 없습니다. 하이픈을 사용하여 파일 이름의 단어와 숫자를 구분합니다.
- 개발, 구매, 빌드, 문제 해결 등의 구체적인 작업 동사를 사용합니다. 현재 진행형 단어는 사용하지 않습니다.
- 한, 및, 그, 내, 또는 등의 군더더기로 간주되는 단어는 가급적 사용하지 않습니다.
- Markdown 파일 및 .md 파일 확장명을 사용해야 합니다.
- 파일 이름은 비교적 짧게 유지합니다. 문서의 URL에 파일 이름이 포함되기 때문입니다.

## <a name="headings"></a>제목

문장 스타일 대/소문자를 사용합니다. 다음 항목은 항상 대문자로 표기합니다.

- 제목의 첫단어
- 제목에서 콜론 뒤에 오는 단어(예: “방법: 배열 정렬”)입니다.

제목은 atx 스타일로 작성해야 합니다. 즉, 줄 시작 부분에서 HTML 제목 수준 H1~H6에 해당하는 1~6개의 해시 문자(#)를 사용하여 제목을 나타냅니다. 위에서는 수준 1/수준 2 헤더의 예가 사용되었습니다.

항목에는 수준 1 제목(H1)이 **하나만** 있어야 합니다. 이 제목이 페이지의 제목으로 표시됩니다.

제목이 `#` 문자로 끝나는 경우에는 제목이 올바르게 렌더링되도록 끝에 여분의 `#` 문자를 추가해야 합니다. 예: `# Async Programming in F# #`.

수준 2 제목은 페이지 제목 아래의 "문서 내용" 섹션에 표시되는 페이지 TOC를 생성합니다.

### <a name="third-level-heading"></a>수준 3 제목
#### <a name="fourth-level-heading"></a>수준 4 제목
##### <a name="fifth-level-heading"></a>수준 5 제목
###### <a name="sixth-level-heading"></a>수준 6 제목

## <a name="text-styling"></a>텍스트 스타일 지정

*기울임꼴* 파일, 폴더, 경로(긴 항목의 경우 자체 줄로 분할), 새 용어, URL(기본 설정인 링크로 렌더링되지 않는 경우)에 사용됩니다.

**굵은 글씨체** UI 요소에 사용됩니다.

## <a name="links"></a>링크

### <a name="internal-links"></a>내부 링크

같은 Markdown 파일의 헤더(앵커 링크라고도 함)에 연결하려면 연결할 헤더의 ID를 확인해야 합니다. ID를 확인하려면 렌더링된 문서의 소스를 확인하여 헤더의 ID(예: `id="blockquote"`)를 찾은 다음 `#blockquote`와 같이 # + ID를 사용하여 연결합니다.
ID는 헤더 텍스트에 따라 자동 생성됩니다. 예를 들어 이름이 `## Step 2`인 고유 섹션의 경우 ID는 `id="step-2"`입니다.

- 예: `[Chapter 1](#chapter-1)`

같은 리포지토리의 Markdown 파일에 연결하려면 파일 이름 끝의 ".md"를 포함한 [상대 링크](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2)를 사용합니다.

- 예: `[Readme file](../readme.md)`
- 예: `[Welcome to .NET](../docs/welcome.md)`

같은 리포지토리에 있는 Markdown 파일의 헤더에 연결하려면 상대 링크 + 해시 태그 링크를 사용합니다.

- 예: `[.NET Community](../docs/welcome.md#community)`

### <a name="external-links"></a>외부 링크

외부 파일에 연결하려면 링크로 전체 URL을 사용합니다.

- 예: `[GitHub](http://www.github.com)`

URL은 Markdown 파일에 표시되는 경우 클릭 가능한 링크로 변환됩니다.

- 예: `http://www.github.com`

### <a name="links-to-apis"></a>API에 대한 링크

빌드 시스템에는 외부 링크를 사용하지 않고도 .NET API에 연결할 수 있는 몇 가지 확장이 포함되어 있습니다.
API에 연결할 때는 소스 코드에서 자동 생성되는 해당 UID(고유 식별자)를 사용할 수 있습니다.

다음 구문 중 하나를 사용할 수 있습니다.

1. Markdown 링크: `[link_text](xref:UID)`
2. 자동 링크: `<xref:UID>`
3. 축약형: `@UID`

- 예: `@System.String`
- 예: `[String class](xref:System.String)`

이 표기법을 사용하는 방법에 대한 자세한 내용은 [상호 참조 사용](https://dotnet.github.io/docfx/tutorial/links_and_cross_references.html#using-cross-reference)을 참조하세요.

> 현재로서는 쉽게 UID를 찾을 수 있는 방법이 없습니다. API의 UID를 찾는 가장 효율적인 방법은 [docascode/coreapi](https://github.com/docascode/coreapi) 리포지토리에서 UID를 검색하는 것입니다. Microsoft는 향후 보다 나은 시스템을 제공하기 위한 작업을 진행하고 있습니다.

UID에 특수 문자를 \` 또는 \#이 포함되어 있으면 다음 예제에서와 같이 UID 값을 각각 %60 및 %23으로 HTML 인코딩해야 합니다.

- 예: @System.Threading.Tasks.Task\`1은 `@System.Threading.Tasks.Task%601`이 됩니다.
- 예: @System.Exception.\#ctor은 `@System.Exception.%23ctor`이 됩니다.

## <a name="lists"></a>목록

### <a name="ordered-lists"></a>순서가 지정된 목록

1. This
1. 예
1. 입니다.
1. Ordered
1. 목록

#### <a name="ordered-list-with-an-embedded-list"></a>포함된 목록이 있는 순서가 지정된 목록

1. 여기
1. 에
1. an
1. 포함된
    1. Miss Scarlett
    1. Professor Plum
1. ordered
1. list

### <a name="unordered-lists"></a>순서가 지정되지 않은 목록

- This
- is
- a
- 글머리 기호
- list

#### <a name="unordered-list-with-an-embedded-list"></a>포함된 목록이 있는 순서가 지정되지 않은 목록

- This
- 글머리 기호
- list
  - 유유진
  - 주준서
- 포함
- 기타
    1. 유현기 대령
    1. 안예은
- 목록

## <a name="horizontal-rule"></a>가로줄

---

## <a name="tables"></a>Tables

| Tables        | 멋진           | 표  |
| ------------- |:-------------:| -----:|
| 3열은      | 오른쪽 맞춤 | $1600 |
| 2열은      | 가운데 맞춤      |   $12 |
| 1열은 기본값 | 왼쪽 맞춤     |    $1 |

[Markdown 테이블 생성기 도구](http://www.tablesgenerator.com/markdown_tables)를 사용하면 테이블을 보다 쉽게 만들 수 있습니다.

### <a name="inline-code-blocks-with-language-identifier"></a>언어 식별자가 포함된 인라인 코드 블록

3개의 backtick(\`\`\`) + 언어 ID를 사용항여 코드 블록에 언어별 색 코딩을 적용합니다. [GFM 언어 ID](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs)의 전체 목록은 다음과 같습니다.

#### <a name="c9839"></a>C&#9839;

```c#
using System;
namespace HelloWorld
{
    class Hello
    {
        static void Main()
        {
            Console.WriteLine("Hello World!");

            // Keep the console window open in debug mode.
            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
}
```

#### <a name="xml"></a>Xml

```xml
<dict>
    <key>CFBundleURLSchemes</key>
    <array>
        <string>evolveCountdown</string>
    </array>
```

#### <a name="xaml"></a>XAML

```xaml
<Label Text="Hello, XAML!"
        FontSize="Large"
        FontAttributes="Bold"
        TextColor="Blue" />
```

#### <a name="powershell"></a>PowerShell

```powershell
Clear-Host
$Directory = "C:\Windows\"
$Files = Get-Childitem $Directory -recurse -Include *.log `
-ErrorAction SilentlyContinue
```

### <a name="generic-code-block"></a>제네릭 코드 블록

제네릭 코드 블록 코딩에는 3개의 backtick(&#96;&#96;&#96;)을 사용합니다.

> 이 경우 설명서 사이트에서 적절한 구문이 강조 표시되도록 이전 섹션에서 설명한 것처럼 언어 식별자가 포함된 코드 블록을 사용하는 것이 좋습니다. 제네릭 코드 블록은 필요할 때만 사용하세요.

```
function fancyAlert(arg) {
    if(arg) {
        $.docs({div:'#foo'})
    }
}
```

### <a name="inline-code"></a>인라인 코드

`inline code`에 대한 backtick(&#95;)을 사용합니다. 인라인 코드는 명령줄 명령, 데이터베이스 테이블/열 이름, 및 언어 키워드에 사용합니다.

## <a name="blockquotes"></a>blockquote

> 가뭄이 천만 년이나 계속되어 끔찍한 도마뱀이 다스리던 시대도 끝이 났습니다. 후세에는 아프리카라고 불릴 대륙인 이곳 적도에서는 생존을 위한 싸움이 극에 달했으며 아직은 누구도 승리하지 못했습니다. 이 황량하고 말라빠진 땅에서는 작고 날쌔며 사나운 생명체만이 살아남거나, 살아남을 수 있다는 희망이라도 품을 수 있습니다.

## <a name="images"></a>이미지

### <a name="static-image-or-animated-gif"></a>정적 이미지 또는 애니메이션 gif

```
![this is the alt text](images/dotnet.png)
```

![반응이 빠른 디자인](images/responsivedesign.gif)

### <a name="linked-image"></a>연결된 이미지

```
[![alt text for linked image](images/dotnet.png)](https://dot.net)
```

## <a name="videos"></a>비디오

### <a name="channel-9"></a>Channel 9

다음 특수 blockquote 구문을 사용합니다.

```
> [!Video https://channel9.msdn.com/Shows/On-NET/Shipping-NET-Core-RC2--Tools-Preview-1/player]
```

Channel 9 비디오 URL은 `https`로 시작하고 `/player`로 끝나야 합니다. 그렇지 않으면 비디오만이 아닌 전체 페이지를 포함하게 됩니다.

### <a name="youtube"></a>YouTube

다음 특수 blockquote 구문을 사용합니다.

```
> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]
```

YouTube 비디오에는 `/embed/`(위의 형식 적용)가 포함되어야 합니다. 그렇지 않으면 전체 페이지를 렌더링하려고 시도하고 작동하지 않을 수 있습니다.

## <a name="docsmicrosoft-extensions"></a>docs.microsoft 확장

docs.microsoft는 GitHub Flavored Markdown에 대해 몇 가지 추가 확장을 제공합니다.

### <a name="alerts"></a>경고

설명서 사이트에서 적절한 스타일로 렌더링되도록 다음 경고 스타일을 사용해야 합니다. 그러나 GitHub의 렌더링 엔진은 스타일을 구분하지 않습니다.

#### <a name="note"></a>참고

```
> [!NOTE]
> This is a NOTE
```

#### <a name="warning"></a>Warning

```
> [!WARNING]
> This is a WARNING
```

#### <a name="tip"></a>팁

```
> [!TIP]
> This is a TIP
```

#### <a name="important"></a>중요

```
> [!IMPORTANT]
> This is IMPORTANT
```

그러면 이러한 항목은 다음과 같이 렌더링됩니다.

![경고 스타일](images/alerts.png)

### <a name="buttons"></a>단추

```
> [!div class="button"]
[button links](../docs/core/index.md)
```

[Intune 문서](https://docs.microsoft.com/intune/get-started/choose-how-to-enroll-devices)에서 작동 중인 단추의 예를 확인할 수 있습니다.

### <a name="selectors"></a>선택기

```
> [!div class="op_single_selector"]
- [macOS](../docs/core/tutorials/using-on-macos.md)
- [Windows](../docs/core/tutorials/using-on-windows.md)
```

[Intune 문서](https://docs.microsoft.com/intune/deploy-use/what-to-tell-your-end-users-about-using-microsoft-intune#how-your-end-users-get-their-apps)에서 작동 중인 선택기의 예를 확인할 수 있습니다.

### <a name="step-by-steps"></a>단계별 작업

```
> [!div class="step-by-step"]
[Pre](../docs/csharp/expression-trees-interpreting.md)
[Next](../docs/csharp/expression-trees-translating.md)
```

[Advanced Threat Analytics 문서](https://docs.microsoft.com/advanced-threat-analytics/deploy-use/install-ata-step2)에서 작동 중인 단계별 작업의 예를 확인할 수 있습니다.
