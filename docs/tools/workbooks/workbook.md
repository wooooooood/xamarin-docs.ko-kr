---
title: 대화형 통합 문서
description: 이 문서에서는 Xamarin Workbooks를 사용 하 여 포함 된 라이브 문서를 만드는 방법을 설명 C# 실험, 교육, 학습 또는 탐색에 대 한 코드입니다.
ms.prod: xamarin
ms.assetid: B79E5DE9-5389-4691-9AA3-FF4336CE294E
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 71f46535ffd0a99ad78acb8f0e3bbc5870abf33e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116760"
---
# <a name="interactive-workbooks"></a>대화형 통합 문서

IDE에서 별도 독립 실행형 응용 프로그램 통합 문서를 사용할 수 있습니다.

새 통합 문서 만들기를 시작 하려면 통합 문서 앱을 실행 합니다. 이 이미 설치 하지 않은, 경우 방문 합니다 [설치](~/tools/workbooks/install.md#install) 페이지입니다. 실시간으로 문서를 시각화할 수 있도록 에이전트 앱을 자동으로 연결 해 선택한 플랫폼에서 통합 문서를 만들라는 메시지가 표시 됩니다.

Workbooks 앱 이미 실행 중인 경우으로 이동 하 여 새 문서를 만들 수 있습니다 **파일 > 새로 만들기**합니다.

통합 문서를 저장 하 고 응용 프로그램 내에서 나중에 다시 열 수 있습니다. 있습니다 수 다음 다른 사용자와 공유할 아이디어를 보여 줍니다., 새로운 Api를 탐색 하거나 새로운 개념을 설명 합니다.

## <a name="code-editing"></a>코드 편집

코드 편집기의 창 코드 완성, 구문 색 지정, 인라인 라이브 진단 및 여러 줄 문 지원에 제공 합니다.

[ ![](workbook-images/inspector-0.6.0-repl-small.png "코드 편집기의 창 코드 완성, 구문 색 지정, 인라인 라이브 진단 및 여러 줄 문 지원 제공")](workbook-images/inspector-0.6.0-repl.png#lightbox)

Xamarin Workbooks에 저장 됩니다는 `.workbook` 파일 맨 위에 있는 일부 메타 데이터를 사용 하 여 CommonMark 파일인 (참조 [통합 문서 파일 형식](#workbooks-files-types) 통합 문서를 저장할 수 있습니다 하는 방법에 대 한 자세한 내용은).

### <a name="nuget-package-support"></a>NuGet 패키지 지원

여러 인기 있는 NuGet 패키지는 Xamarin Workbooks에서 직접 지원 됩니다. 로 이동 하 여 패키지를 검색할 수 있습니다 **파일 > 패키지 추가**합니다. 를 자동으로 가져오므로 `#r` 문 바로 사용할 수 있도록 패키지 어셈블리를 참조 합니다.

패키지 참조를 사용 하 여 통합 문서를 저장 하는 경우 이러한 참조도 저장 됩니다. 다른 사용자와의 통합 문서를 공유 하는 경우 참조 된 패키지를 자동으로 다운로드 됩니다.

NuGet 패키지 지원 통합 문서에서 몇 가지 알려진된 제한 사항이 있습니다.

- 네이티브 라이브러리 iOS에만 지원 되며 관리 되는 라이브러리를 사용 하 여 연결 하는 경우에 있습니다.
- 패키지에 종속 된 `.targets` 예상 대로 작동 하도록 파일 또는 PowerShell 스크립트는 실패할 수 있습니다.
- 를 제거 하거나 패키지 종속성을 수정 하려면 텍스트 편집기를 사용 하 여 통합 문서의 매니페스트를 편집 합니다. 적절 한 패키지 관리 방식으로 켜져 있습니다.

### <a name="xamarinforms-support"></a>Xamarin.Forms 지원

통합 문서에서 Xamarin.Forms NuGet 패키지를 참조 하는 경우 통합 문서 앱 Xamarin.Forms 기반 기본 뷰를 변경 됩니다. 통해 액세스할 수 있습니다 `Xamarin.Forms.Application.Current.MainPage`합니다.

보기 관리자 탭의 레이아웃을 이해 하기 위해 Xamarin.Forms 뷰 계층 구조를 표시 하는 데 특별 한 지원이 있습니다.

## <a name="rich-text-editing"></a>서식 있는 텍스트 편집

코드를 포함 하 여 서식 있는 텍스트 편집기를 사용 하 여 아래와 같이 텍스트를 편집할 수 있습니다.

![](workbook-images/inspector-0.6.2-editing.gif "기본 제공 된 서식 있는 텍스트 편집기를 사용 하는 코드 주위에 텍스트를 편집 합니다.")

### <a name="markdown-authoring"></a>Markdown 작성

통합 문서 작성자 경우가 쉽게 직접 해당 즐겨 찾는 편집기를 사용 하 여 CommonMark "source" 통합 문서를 편집할 수 있습니다.

편집 하려는 경우 다음 통합 문서 클라이언트 내에서 통합 문서를 저장, CommonMark 텍스트는 변경 될 수 있습니다에 유의 합니다.

참고 CommonMark 확장으로 인해 사용 하 여 통합 문서 파일에서 YAML 메타 데이터를 사용 하도록 설정 하려면 `---` 해당 목적을 위해 예약 되어 있습니다. 만들려는 경우 [원한다 면 나누기](http://spec.commonmark.org/0.27/#thematic-break) 텍스트를 사용 해야 `***` 또는 `___` 대신 합니다. 통합 문서 1.2에 저장 하는 동안 버그로 인해 이전에 이러한 중단을 피해 야 합니다.

### <a name="improvements-in-workbooks-13"></a>통합 문서 1.3의에서 향상 된 기능

Markdown 블록 따옴표 구문은 프레젠테이션을 개선 하기 위해 약간 확장 했습니다. 블록 견적의 첫 번째 문자로이 모 지를 더하여 견적의 배경색 영향을 줄 수 있습니다.

- `> [!NOTE]`
    > 파란색 배경의 참고도 렌더링 됩니다.
- `> [!IMPORTANT]`
    > 배경이 노란색인 경고도 렌더링 됩니다.
- `> [!WARNING]`
    > 빨간색 배경의 문제가으로 렌더링 됩니다.

통합 문서의 헤더에 연결할 수 있습니다. 머리글 텍스트를 다음과 같이 처리 되 고 앵커 ID를 사용 하 여 각 헤더에 대 한 앵커 생성 합니다.

1. 헤더는 소문자로 변환 합니다.
1. 영숫자 및 대시를 제외한 모든 문자가 제거 됩니다.
1. 모든 공백이 대시를 사용 하 여 대체 됩니다.

즉 헤더를 "중요 한 헤더"의 id를 가져옵니다와 같은 `important-header` 에 대 한 링크를 삽입 하 여 연결할 수 있습니다 및 `#important-header` 통합 문서에 있습니다.

## <a name="document-structure"></a>문서 구조

### <a name="cell"></a>셀

실행 코드 또는 markdown 나타내는 콘텐츠의 불연속 단위입니다. 코드 셀을 최대 4 개의 하위 구성 요소의 구성 됩니다.

- 편집기
  - 버퍼
- 컴파일러 진단
- 콘솔 출력
- 실행 결과

### <a name="editor"></a>편집기

셀의 대화형 텍스트 구성 요소입니다. 코드 셀에 대 한 구문 강조 표시 등을 사용 하 여 실제 코드 편집기입니다. Markdown 셀에 대 한 상황에 맞는 서식 지정 및 작성 도구 모음을 사용 하 여 서식 있는 텍스트 콘텐츠 편집기입니다.

### <a name="buffer"></a>버퍼

편집기의 실제 텍스트 내용입니다.

### <a name="compiler-diagnostics"></a>컴파일러 진단

진단 명시적 실행은 요청 하는 경우에 렌더링 코드를 컴파일할 때 생성 됩니다. 셀 편집기 아래에 즉시 표시 합니다.

### <a name="console-output"></a>콘솔 출력

셀을 실행 하는 동안 표준 출력 또는 표준 오류를 쓸 출력입니다. 검은색 텍스트로 렌더링할 표준 출력 및 표준 오류는 빨간색 텍스트로 렌더링 됩니다.

### <a name="execution-results"></a>실행 결과

결과 실제로 실행 하 여 생성 되는 제공 컴파일이 성공 하면 결과 셀에 대 한 풍부 하 고 잠재적으로 대화형 표현은 렌더링 됩니다. 예외는 컴파일에 실제로 실행 한 결과로 생성 될 때 이므로이 컨텍스트에서 결과 간주 됩니다.

## <a name="workbooks-files-types"></a>통합 문서 파일 형식

### <a name="plain-files"></a>일반 파일

기본적으로 통합 문서는 일반 텍스트로 저장 `.workbook` CommonMark 서식 있는 텍스트를 포함 하는 파일입니다.

### <a name="packages"></a>패키지

통합 문서 패키지는 이름이 지정 된 디렉터리는 `.workbook` 확장 합니다.
Mac의 찾기 및 Xamarin Workbooks 열기 대화 상자 및 최근 파일 메뉴를 파일로 것 처럼이 디렉터리 인식 됩니다.

디렉터리에 포함 해야 합니다는 `index.workbook` Xamarin Workbooks에서 로드 되는 실제 텍스트로 통합 문서 파일. 디렉터리에 필요한 리소스를 포함할 수도 있습니다 `index.workbook`, 이미지 또는 기타 파일을 포함 합니다.

경우 일반 텍스트 `.workbook` 0.99.3 통합 문서에서 동일한 디렉터리에서 리소스를 참조 하는 파일을 열거나 나중에 저장 될 때 변환에 `.workbook` 패키지 있습니다. Mac과 Windows 모두에서 그렇습니다.

> [!NOTE]
> Windows 사용자 열립니다는 `package.workbook\index.workbook` 직접 파일 있지만 그렇지 않은 경우 패키지는 동일 하 게 mac에서 작업 하는 대로

### <a name="archives"></a>보관 파일

통합 문서 패키지 디렉터리 되는 인터넷을 통해 쉽게 배포할 어려울 수 있습니다. 솔루션은 통합 문서 보관 합니다. 통합 문서 보관은 이라는 통합 문서가 zip 압축 된 패키지에는 `.workbook` 확장 합니다.

통합 패키지를 저장 하는 경우 통합 문서 1.1부터 저장 대화 상자는 선택으로 저장 하는 대신 제공 합니다. 1.0 통합 문서에 만들거나 보관 파일을 저장 하는 기본 제공 방법이 없었습니다.

통합 문서 보관 파일을 열 때 통합 문서 패키지로 투명 하 게 변환 된 통합 문서 1.0 및 zip 파일이 손실 되었습니다. 통합 문서 1.1에서는 zip 파일 유지 됩니다. 사용자는 보관 파일을 저장 하는 경우 새 zip 파일을 사용 하 여 대체 됩니다.

통합 문서 패키지를 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 통합 문서 보관 파일을 수동으로 만들 수 있습니다 **압축** Mac에서 또는 **보내기 > 압축 (zip) 폴더** Windows에서. 다음 zip 파일의 이름을 바꿉니다는 `.workbook` 파일 확장명입니다. 이 통합 문서 패키지에 없습니다 일반 통합 문서 파일을 사용 하 여 작동합니다.

## <a name="related-links"></a>관련 링크

- [통합 문서에 시작](https://developer.xamarin.com/workbooks/workbooks/getting-started/welcome.workbook)
