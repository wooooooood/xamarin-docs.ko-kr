---
title: 대화형 통합 문서
description: 교육 또는 탐색 교육, 실험에 대 한 C# 코드를 사용 하 여 라이브 문서를 만들려면 통합 문서를 사용 합니다.
ms.prod: xamarin
ms.assetid: B79E5DE9-5389-4691-9AA3-FF4336CE294E
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 7577380ff78b9b94b88f5a4190df32400d2c573f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="interactive-workbooks"></a>대화형 통합 문서

_교육 또는 탐색 교육, 실험에 대 한 C# 코드를 사용 하 여 라이브 문서를 만들려면 통합 문서를 사용 합니다._

독립 실행형 응용 프로그램으로, IDE에서 별도 통합 문서를 사용할 수 있습니다.

새 통합 문서 만들기를 시작 하려면 통합 문서 앱을 실행 합니다. 이 이미 설치 하지 않은, 방문는 [설치](~/tools/workbooks/install.md#install) 페이지. 실시간으로 문서를 시각화할 수 있도록 에이전트 앱에 자동으로 연결 해 선택한 플랫폼에서 통합 문서를 만들려면 묻는 메시지가 나타납니다.

통합 문서 앱 이미 실행 중인 경우로 이동 하 여 새 문서를 만들 수 있습니다 **파일 > 새**합니다.

통합 문서 저장 하 고 응용 프로그램 내에서 나중에 다시 열 수 있습니다. 있습니다 수 다음 다른 사용자와 공유할를 아이디어를 시연, 새로운 Api를 탐색 하거나 새로운 개념을 설명 합니다.

## <a name="code-editing"></a>코드 편집

코드 편집 창 코드 완성, 구문 색 지정, 인라인 라이브 진단 및 여러 줄으로 된 문 지원에 제공 합니다.

[ ![](workbook-images/inspector-0.6.0-repl-small.png "코드 편집 창 제공 코드 완성, 구문 색 지정, 인라인 라이브 진단 및 여러 줄으로 된 문 지원")](workbook-images/inspector-0.6.0-repl.png#lightbox)

Xamarin 통합 문서에 저장 됩니다는 `.workbook` 는 CommonMark 일부 메타 데이터가 있는 파일 위쪽에 있는 파일 (참조 [통합 문서 파일 형식을](#workbooks-files-types) 통합 문서를 저장할 수 방법에 대 한 자세한 내용은).

### <a name="nuget-package-support"></a>NuGet 패키지 지원

많은 인기 있는 NuGet 패키지 Xamarin 통합 문서에서 직접 사용할 수 있습니다. 로 이동 하 여 패키지를 검색할 수 있습니다 **파일 > 패키지 추가**합니다. 를 자동으로 가져오므로 `#r` 문 바로 사용할 수 있도록 패키지 어셈블리를 참조 합니다.

패키지 참조와 통합 문서를 저장 하는 경우 해당 참조도 저장 됩니다. 다른 사용자와 통합 문서를 공유 하는 경우 참조 된 패키지가 자동으로 다운로드 됩니다.

통합 문서에서 NuGet 패키지를 지 원하는 몇 가지 알려진된 제한 사항이 있습니다.

  * 네이티브 라이브러리 iOS에만 지원 되며 관리 되는 라이브러리와 연결 된 경우에 가능 합니다.
  * 패키지에 따라 달라 지는 `.targets` 파일 또는 PowerShell 스크립트 예상 대로 작동 하도록 삭제가 실패 합니다.
  * 을 제거 하거나 패키지 종속성을 수정 하려면 텍스트 편집기로 통합 문서의 매니페스트를 편집 합니다. 적절 한 패키지 관리 방식에는입니다.

### <a name="xamarinforms-support"></a>Xamarin.Forms Support

통합 문서에서 Xamarin.Forms NuGet 패키지를 참조 하는 경우 통합 문서 응용 프로그램의 주 보기를 Xamarin.Forms 기반 변경 됩니다. 통해 액세스할 수 `Xamarin.Forms.Application.Current.MainPage`합니다.

보기 검사기 탭에는 다음과 같은 레이아웃을 쉽게 이해할 수 있도록 Xamarin.Forms 뷰 계층 구조를 표시 하는 데 특별 한 지원을 합니다.

## <a name="rich-text-editing"></a>서식 있는 텍스트 편집

아래 그림과 같이, 서식 있는 텍스트 편집기를 사용 하 여 코드 주위 텍스트를 편집할 수 있습니다.

![](workbook-images/inspector-0.6.2-editing.gif "기본 제공 된 서식 있는 텍스트 편집기를 사용 하 여 코드 주위에 텍스트를 편집 합니다.")

### <a name="markdown-authoring"></a>마크 다운 작성

통합 문서 만든 경우가 쉽게 선호 하는 편집기와 CommonMark "source" 통합 문서를 직접 편집할 수 있습니다.

주의 다음 편집 하 고 통합 문서 클라이언트 내에서 통합 문서를 저장, CommonMark 텍스트는 포맷 수 수 있습니다.

CommonMark 확장으로 인해 사용 하 여 YAML 메타 데이터 통합 문서 파일에 사용할 수 있도록 점에 유의 하십시오 `---` 는 해당 용도로 예약 되어 있습니다. 만들려고 하는 경우 [주제 나누기](http://spec.commonmark.org/0.27/#thematic-break) 텍스트에 사용 해야 `***` 또는 `___` 대신 합니다. 통합 문서 1.2에서 및 저장 하는 동안 버그로 인해 이전 이러한 나누기 피해 야 합니다.

### <a name="improvements-in-workbooks-13"></a>통합 문서 1.3의에서 향상 된 기능

표시를 개선 하는 약간 Markdown 블록 견적 구문을 확장 했습니다. emoji 블록 따옴표의 첫 번째 문자를 추가 하 여는 따옴표의 배경색을 달라질 수 있습니다.

- `> [!NOTE]
>' 파란색 배경의 참고로 렌더링 될
- `> [!IMPORTANT]
>' 경고는 노란색 배경의 렌더링 합니다
- `> [!WARNING]
>' 문제가와 빨강 배경색으로 렌더링 합니다

통합 문서에 헤더를 연결할 수도 있습니다. 헤더 텍스트를 다음과 같이 처리 되 고 앵커 ID와 각 헤더에 대 한 앵커 생성:

1. 헤더가 아래 대/소문자를 사용 합니다.
1. 영숫자, 대시를 제외한 모든 문자가 제거 됩니다.
1. 모든 공간 파선으로 대체 됩니다.

즉, 머리글 "중요 한 헤더"의 id를 가져옵니다. 같은 `important-header` 에 대 한 링크를 삽입 하 여 연결할 수 `#important-header` 통합 문서에 있습니다.

## <a name="document-structure"></a>문서 구조

### <a name="cell"></a>셀

별개의 단위로 콘텐츠를 나타내는 실행 코드 또는 마크 다운 합니다. 최대 4 개의 하위 구성 요소 코드 셀 구성 됩니다.

- 편집기
  - 버퍼
- 컴파일러 진단
- 콘솔 출력
- 실행 결과

### <a name="editor"></a>편집기

셀의 대화형 텍스트 구성 요소입니다. 코드 셀 실제 코드 편집기에서 구문 강조, 등입니다. 마크 다운 셀 서식 있는 텍스트 콘텐츠 편집기 상황에 맞는 서식 지정 및 제작 도구 모음입니다.

### <a name="buffer"></a>버퍼
편집기의 실제 텍스트 콘텐츠입니다.

### <a name="compiler-diagnostics"></a>컴파일러 진단

진단에는 명시적인 실행을 요청 하는 경우에 렌더링 코드를 컴파일할 때 발생 합니다. 셀 편집기 아래에 즉시 표시 합니다.

### <a name="console-output"></a>콘솔 출력

모든 출력 셀의 실행 하는 동안 표준 제한 또는 표준 오류 로그에 기록 합니다. 검은색 텍스트로 렌더링 되는 표준 출력 및 표준 오류는 빨간색 텍스트로 렌더링 됩니다.

### <a name="execution-results"></a>실행 결과

결과 실행 하 여 실제로 생성 되는 제공 컴파일이 성공 시 셀에 대 한 결과의 풍부 하 고 잠재적으로 대화형 표현은 렌더링 됩니다. 예외는 컴파일을 실제로 실행 한 결과로 생성 된 이후이 컨텍스트에서 결과 간주 됩니다.

## <a name="workbooks-files-types"></a>통합 문서 파일 형식

### <a name="plain-files"></a>일반 파일

기본적으로 통합 문서는 일반 텍스트로 저장 `.workbook` CommonMark 형식의 텍스트를 포함 하는 파일입니다.

### <a name="packages"></a>패키지

통합 문서 패키지는 이름이 지정 된 디렉터리는 `.workbook` 확장 합니다.
이 디렉터리에서 Mac의 찾기 및 Xamarin 통합 문서 열기 대화 상자 및 최근 파일 메뉴에 파일 마치 인식 됩니다.

디렉터리에 포함 해야 합니다는 `index.workbook` Xamarin 통합 문서에 로드 되는 실제 일반 텍스트 통합 문서에 있는 파일입니다. 디렉터리에 필요한 리소스를 포함할 수도 있습니다 `index.workbook`, 이미지 또는 기타 파일을 포함 합니다.

경우 일반 텍스트 `.workbook` 0.99.3 통합 문서에서 동일한 디렉터리에서 리소스를 참조 하는 파일을 열거나 나중 저장 될 때 변환 됩니다에 `.workbook` 패키지 합니다. Mac 및 Windows 둘 다에 유용합니다.

> [!NOTE]
> Windows 사용자가을 열는 `package.workbook\index.workbook` 를 직접 파일 있지만 그렇지 않은 경우 패키지 Mac.에서와 동일 하 게 동작 합니다

### <a name="archives"></a>보관 파일

패키지 통합 문서, 디렉터리, 되 고 인터넷을 통해 쉽게 배포할 어려울 수 있습니다. 솔루션은 통합 문서 보관 합니다. 통합 문서 보관은 명명 된 하는 통합 문서 zip 압축 된 패키지는 `.workbook` 확장 합니다.

통합 문서 패키지를 저장할 때 통합 문서 1.1 부터는 저장 대화에서는 대신 보관으로 저장 합니다. 통합 문서 1.0 했습니다 만들기 또는 보관 파일을 저장할 기본 제공 방법이 없습니다.

통합 문서 1.0에서 통합 문서 보관 파일을 열 때 통합 문서 패키지로 투명 하 게 변환 된 및 zip 파일이 손실 되었습니다. 통합 문서 1.1 zip 파일은 없습니다. 사용자는 보관 파일을 저장 하는 경우 새 zip 파일로 바뀝니다.

통합 문서 패키지를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 통합 문서 보관 파일을 수동으로 만들 수 있습니다 **압축** mac, 또는 **보내기 > 압축 (zip) 폴더** Windows에서 합니다. 다음을 zip 파일 이름을 한 `.workbook` 파일 확장명입니다. 이 통합 문서 패키지 하지 일반 통합 문서 파일 에서만 작동합니다.

## <a name="related-links"></a>관련 링크

- [통합 문서에 시작](https://developer.xamarin.com/workbooks/workbooks/getting-started/welcome.workbook)
