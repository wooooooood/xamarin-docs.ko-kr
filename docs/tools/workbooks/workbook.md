---
title: 대화형 Workbooks
description: 이 문서에서는 Xamarin Workbooks를 사용 하 여 실험, 학습, C# 학습 또는 탐색을 위한 코드를 포함 하는 라이브 문서를 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B79E5DE9-5389-4691-9AA3-FF4336CE294E
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 7b3c356460d9427821843dc084b3f306c026ffa0
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70293082"
---
# <a name="interactive-workbooks"></a>대화형 Workbooks

통합 문서를 IDE와 분리 된 독립 실행형 응용 프로그램으로 사용할 수 있습니다.

새 통합 문서 만들기를 시작 하려면 통합 문서 앱을 실행 합니다. 아직 설치 하지 않은 경우 [설치](~/tools/workbooks/install.md#install) 페이지를 방문 하세요. 선택한 플랫폼에서 통합 문서를 만들라는 메시지가 표시 됩니다. 그러면 에이전트 앱에 자동으로 연결 되어 실시간으로 문서를 시각화할 수 있습니다.

통합 문서 응용 프로그램이 이미 실행 중인 경우 **파일 > 새로**만들기를 검색 하 여 새 문서를 만들 수 있습니다.

통합 문서를 저장 하 고 나중에 응용 프로그램 내에서 다시 열 수 있습니다. 그런 다음 다른 사용자와 공유 하 여 아이디어를 시연 하거나 새 Api를 탐색 하거나 새로운 개념을 배울 수 있습니다.

## <a name="code-editing"></a>코드 편집

코드 편집 창은 코드 완성, 구문 색 지정, 인라인 라이브 진단 및 여러 줄 문 지원 기능을 제공 합니다.

[![](workbook-images/inspector-0.6.0-repl-small.png "코드 편집 창에서는 코드 완성, 구문 색 지정, 인라인 라이브 진단 및 여러 줄 문 지원 기능을 제공 합니다.")](workbook-images/inspector-0.6.0-repl.png#lightbox)

Xamarin Workbooks는 `.workbook` 파일에 저장 됩니다 .이 파일은 맨 위에 일부 메타 데이터가 있는 CommonMark 파일입니다 (통합 문서를 저장 하는 방법에 대 한 자세한 내용은 [통합 문서 파일 형식](#workbooks-files-types) 참조).

### <a name="nuget-package-support"></a>NuGet 패키지 지원

널리 사용 되는 많은 NuGet 패키지는 Xamarin Workbooks에서 직접 지원 됩니다. **파일 > 패키지 추가**로 이동 하 여 패키지를 검색할 수 있습니다. 를 자동으로 가져오므로 `#r` 문 바로 사용할 수 있도록 패키지 어셈블리를 참조 합니다.

패키지 참조가 포함 된 통합 문서를 저장 하면 해당 참조도 저장 됩니다. 다른 사용자와 통합 문서를 공유 하는 경우 참조 된 패키지를 자동으로 다운로드 합니다.

통합 문서에는 NuGet 패키지 지원에 대 한 몇 가지 알려진 제한 사항이 있습니다.

- 네이티브 라이브러리는 iOS 에서만 지원 되며 관리 되는 라이브러리와 연결 된 경우에만 지원 됩니다.
- 파일이 나 PowerShell 스크립트 `.targets` 에 종속 된 패키지는 예상 대로 작동 하지 않을 수 있습니다.
- 패키지 종속성을 제거 하거나 수정 하려면 텍스트 편집기를 사용 하 여 통합 문서의 매니페스트를 편집 합니다. 적절 한 패키지 관리는 적절 한 방법입니다.

### <a name="xamarinforms-support"></a>Xamarin.ios 지원

통합 문서에서 Xamarin.ios NuGet 패키지를 참조 하는 경우 통합 문서 앱은 기본 보기를 Xamarin. Forms 기반으로 변경 합니다. 를 통해 `Xamarin.Forms.Application.Current.MainPage`액세스할 수 있습니다.

뷰 검사기 탭에는 레이아웃을 이해 하는 데 도움이 되는 Xamarin.ios 뷰 계층 구조를 표시할 수 있는 특별 한 지원도 있습니다.

## <a name="rich-text-editing"></a>서식 있는 텍스트 편집

아래 그림과 같이 서식 있는 텍스트 편집기를 사용 하 여 코드 주위의 텍스트를 편집할 수 있습니다.

![](workbook-images/inspector-0.6.2-editing.gif "기본 제공 서식 있는 텍스트 편집기를 사용 하 여 코드 주위의 텍스트 편집")

### <a name="markdown-authoring"></a>Markdown 작성

통합 문서 작성자는 때때로 즐겨 사용 하는 편집기를 사용 하 여 통합 문서의 CommonMark "원본"을 직접 편집 하는 것이 더 쉬울 수 있습니다.

통합 문서를 편집 하 고 통합 문서 클라이언트 내에서 저장 하는 경우에는 CommonMark 텍스트의 서식을 다시 지정할 수 있습니다.

통합 문서 파일 `---` 에서 yaml 메타 데이터를 사용 하도록 설정 하는 데 사용 되는 CommonMark 확장으로 인해는 해당 목적을 위해 예약 되어 있습니다. 텍스트에 [테마 나누기](http://spec.commonmark.org/0.27/#thematic-break) 를 만들려면 대신 또는 `***` `___` 를 사용 해야 합니다. 저장 하는 동안 버그로 인해 1.2 이전 버전의 통합 문서에서는 이러한 나누기를 피해 야 합니다.

### <a name="improvements-in-workbooks-13"></a>통합 문서 1.3의 향상 된 기능

또한 프레젠테이션을 개선 하기 위해 Markdown 블록 따옴표 구문을 약간 확장 했습니다. 블록 인용의 첫 번째 문자로 서 수를 추가 하 여 따옴표의 배경색에 영향을 줄 수 있습니다.

- `> [!NOTE]`
    > 파란색 배경의 메모로 렌더링 됩니다.
- `> [!IMPORTANT]`
    > 노란색 배경의 경고로 렌더링 됩니다.
- `> [!WARNING]`
    > 빨간색 배경의 문제로 렌더링 됩니다.

통합 문서 문서에서 헤더에 연결할 수도 있습니다. 각 헤더에 대 한 앵커를 생성 하며, 앵커 ID는 헤더 텍스트가 되며 다음과 같이 처리 됩니다.

1. 이 헤더는 대/소문자를 구분 합니다.
1. 영숫자 및 대시를 제외한 모든 문자가 제거 됩니다.
1. 모든 공백은 대시로 바뀝니다.

즉, "중요 한 헤더"와 같은 헤더는 통합 문서에 `important-header` 에 대 `#important-header` 한 링크를 삽입 하 여의 id를 가져오고 연결할 수 있습니다.

## <a name="document-structure"></a>문서 구조

### <a name="cell"></a>셀

실행 코드 또는 markdown를 나타내는 개별 콘텐츠 단위입니다. 코드 셀은 최대 4 개의 하위 구성 요소로 구성 됩니다.

- 편집기
  - 버퍼
- 컴파일러 진단
- 콘솔 출력
- 실행 결과

### <a name="editor"></a>편집기

셀의 대화형 텍스트 구성 요소입니다. 코드 셀의 경우이는 구문 강조 표시 등의 실제 코드 편집기입니다. Markdown 셀의 경우이는 상황에 맞는 서식 지정 및 제작 도구 모음이 포함 된 서식 있는 텍스트 내용 편집기입니다.

### <a name="buffer"></a>버퍼

편집기의 실제 텍스트 내용입니다.

### <a name="compiler-diagnostics"></a>컴파일러 진단

코드를 컴파일할 때 생성 되는 모든 진단은 명시적 실행이 요청 될 때만 렌더링 됩니다. 셀 편집기 바로 아래에 표시 됩니다.

### <a name="console-output"></a>콘솔 출력

셀을 실행 하는 동안 표준 출력 또는 표준 오류에 쓰여진 모든 출력입니다. 표준 출력은 검은색 텍스트로 렌더링 되 고 표준 오류는 빨간색 텍스트로 렌더링 됩니다.

### <a name="execution-results"></a>실행 결과

결과를 실제로 실행 하 여 생성 하는 경우에는 성공적으로 컴파일할 때 셀에 대 한 풍부 하 고 잠재적으로 대화형으로 표시 되는 결과가 렌더링 됩니다. 예외는 실제로 컴파일을 실행 한 결과로 생성 되기 때문에이 컨텍스트에서 결과로 간주 됩니다.

## <a name="workbooks-files-types"></a>통합 문서 파일 형식

### <a name="plain-files"></a>일반 파일

기본적으로 통합 문서는 CommonMark 형식의 텍스트를 포함 `.workbook` 하는 일반 텍스트 파일로 저장 합니다.

### <a name="packages"></a>패키지

통합 문서 패키지는 `.workbook` 확장명이로 지정 된 디렉터리입니다.
Mac의 Finder 및 Xamarin Workbooks 열기 대화 상자 및 최근 파일 메뉴에서이 디렉터리는 파일 처럼 인식 됩니다.

디렉터리에는 Xamarin Workbooks 로드 `index.workbook` 되는 실제 일반 텍스트 통합 문서인 파일이 포함 되어 있어야 합니다. 디렉터리는 이미지 또는 기타 파일을 비롯 `index.workbook`하 여에 필요한 리소스를 포함할 수도 있습니다.

같은 디렉터리의 리소스 `.workbook` 를 참조 하는 일반 텍스트 파일이 통합 문서 0.99.3 이상에서 열리면 저장 될 때 `.workbook` 패키지로 변환 됩니다. Mac 및 Windows 모두에 적용 됩니다.

> [!NOTE]
> Windows 사용자는 `package.workbook\index.workbook` 파일을 직접 열 수 있습니다. 그렇지 않으면 패키지가 Mac에서와 동일 하 게 동작 합니다.

### <a name="archives"></a>파일과

디렉터리가 되는 통합 문서 패키지는 인터넷을 통해 쉽게 배포 하기가 어려울 수 있습니다. 이 솔루션은 통합 문서 보관입니다. 통합 문서 보관은 `.workbook` 확장명이 인 압축 된 통합 문서 패키지입니다.

통합 문서 1.1부터 통합 문서 패키지를 저장 하는 경우 저장 대화 상자는 대신 보관 파일로 저장 하는 옵션을 제공 합니다. 통합 문서 1.0에는 보관 파일을 만들거나 저장할 수 있는 기본 제공 방법이 없습니다.

통합 문서 1.0에서 통합 문서 보관 파일이 열리면 통합 문서 패키지로 투명 하 게 변환 되 고 zip 파일이 손실 됩니다. 통합 문서 1.1에서 zip 파일은 그대로 유지 됩니다. 사용자는 보관 파일을 저장할 때 새 zip 파일로 대체 됩니다.

통합 문서 패키지를 마우스 오른쪽 단추로 클릭 하 고 Mac에서 **압축** 을 선택 하거나 Windows의 **Zip (> 압축) 폴더로 전송** 하 여 통합 문서 보관 파일을 수동으로 만들 수 있습니다. 그런 다음 `.workbook` 파일 확장명을 포함 하도록 zip 파일의 이름을 바꿉니다. 이는 일반 통합 문서 파일이 아닌 통합 문서 패키지 에서만 작동 합니다.

## <a name="related-links"></a>관련 링크

- [통합 문서 시작](https://developer.xamarin.com/workbooks/workbooks/getting-started/welcome.workbook)
