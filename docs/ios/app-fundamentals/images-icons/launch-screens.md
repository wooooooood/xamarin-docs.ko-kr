---
title: 시작 화면
description: 이 문서에서는 모든 해상도와 방향으로 하나의 통합 된 스토리 보드를 사용 하 여 모든 iOS 장치에 대 한 응용 프로그램 시작 화면을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/19/2018
ms.openlocfilehash: 991c2f30bcca1969e336f7269ad2a22ce6245b95
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="launch-screens"></a>시작 화면

_이 문서에서는 모든 해상도와 방향으로 하나의 통합 된 스토리 보드를 사용 하 여 모든 iOS 장치에 대 한 응용 프로그램 시작 화면을 만드는 방법을 설명 합니다._

전에 iOS 8, iOS 앱에 대 한 시작 화면을 만드는 다양 한 폼 팩터에 모두 장치 및 앱에 실행할 수 있는 해결 방법은 각 이미지 자산의 제공 하는 개발자가 필요 합니다. 그러나 IOS 8 릴리스 이후 되었습니다 모든 경우에에서 문제가 없는 경우 시작 화면을 만들려면 하나의 통합 된 스토리 보드를 사용 하 여 합니다.

이 짧은 연습 새 프로젝트에서 기본적으로 제공 된 Storyboard 또는 기존 프로젝트에 수동으로 추가 하는 스토리 보드는 시작 화면을 만드는 방법을 설명 합니다. 그런 다음 해당 뷰에 대 한 제약 조건을 설정 하 고 스토리 보드 다양 한 장치 및 방향을 제대로 표시 되는지 확인 하려면 스토리 보드에 이미지 보기 및 레이블을 추가 하는 iOS 디자이너를 사용 하는 방법을 보여 줍니다.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>스토리 보드가 있는 관리 시작 화면

IOS 8 이상, 개발자는 하나 이상의 정적 시작 이미지를 사용 하는 대신 시작 화면을 제공 하는 특별 한 통합 스토리를 만들 수 있습니다. 시작 스토리 보드 디자이너의 ios를 만들 때 다른 디스플레이 환경에 대 한 다양 한 레이아웃을 정의할 수 크기 클래스와 자동 레이아웃을 사용 합니다. 크기 클래스 및 자동 레이아웃을 사용 하 여 개발자는 모든 장치에서 제대로 보이는 단일 실행 화면을 만들 수 있으며 환경 표시.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac 용 Visual Studio에서 새 프로젝트를 선택 하 여 만들 **파일 > 새 솔루션** 를 선택한 다음 **단일 앱 보기**: 

    ![선택 하는 단일 보기 응용 프로그램 사용 하 여 새 프로젝트 창](launch-screens-images/launch01.png)

    - 기본적으로 새 프로젝트에 포함 되는 **LaunchScreen.storyboard** 시작 스크린 인터페이스를 정의 하는 파일입니다. 
    - 대신 시작 화면 스토리 보드에 추가할 기존 프로젝트를 마우스 오른쪽 단추로 클릭에서 프로젝트 이름을 **솔루션 패드** 선택 **추가 > 새 파일...**  선택한 후 **시작 화면**:

    ![새 파일 창 ios 선택 시작 화면](launch-screens-images/launch01b.png)

    - 파일 이름을 **LaunchScreen** 또는 다른 사용자가 선택한 이름입니다.

2. 시작 화면에 대 한 적절 한 스토리 보드를 사용 하도록 프로젝트를 구성 합니다.

    - 두 번 클릭는 **Info.plist** 파일에 **솔루션 패드** 를 편집 하기 위해 엽니다.
    - 에 **시작 이미지** 섹션에서 다음 사항을 확인 **시작 화면** 적절 한 스토리 보드의 이름으로 설정 됩니다.

    ![Info.plist의 시작 화면 선택기](launch-screens-images/launch02.png)

    - 기본적으로 새 프로젝트 구성에 사용 하도록 **LaunchScreen.storyboard** 시작 화면으로 합니다.

3. 이미지를 추가할는 **Assets.xcassets** 자산 카탈로그 시작 화면에서 사용 하기 위해 사용할 수 있도록 합니다. 자세한 내용은 참조는 [는 자산 카탈로그 이미지를 설정 하는 이미지 추가](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 의 섹션은 [이미지 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 가이드입니다.

4. 열기 **LaunchScreen.storyboard** 에 두 번 클릭 하 여 편집을 위해는 **솔루션 패드**합니다.

5. 장치 및 방향을 시작 화면에서에서 스토리 보드 디자이너 iOS 미리 보기를 선택 합니다. 아래쪽 도구 모음에서 장치 선택 창을 열고 선택 **iPhone 4S** 및 **세로**합니다.

    ![장치 선택 도구 모음](launch-screens-images/launch05.png)

    - 선택 하면 장치를 방향 iOS 디자이너 디자인을 미리 봅니다. 방법을 변경 합니다. 여기에서 선택 영역에 관계 없이 새로 추가 된 경우가 아니면 모든 장치와 방향을 제약 조건이 적용 됩니다는 **특성 편집** 별도로 지정 하는 데 사용 된 단추입니다. 

6. 설정의 **배경** 보기 컨트롤러의 주 보기의 색입니다. 뷰-컨트롤러에서 중간에 클릭 하 여 보기를 선택 하 고 사용 하 여 배경 색 조정는 **속성 패드**:

    ![자주색 배경색으로 단일 보기](launch-screens-images/launch06.png)

7. 추가 **이미지 보기** 시작 화면으로 해당 소스를 설정 하 고 **이미지**:

    - 끌어서는 **이미지 보기** 에서 **도구 상자 패드** 보기의 가운데에 있습니다.
    - 와 **이미지 보기** 선택한 상태에서 **위젯** 의 섹션은 **속성 패드** 설정는 **이미지** 속성을 이미 이미지 설정 에 추가 된 **Assets.xcassets** 자산 카탈로그입니다. 위치 및 크기는 **이미지 보기** 필요에 따라:
    
    ![이미지 속성 집합과 함께 이미지 보기는](launch-screens-images/launch07.png)

8. 추가 **레이블** 아래는 **이미지 보기** 사용 하는 **속성 패드** 해당 특성을 설정 하려면: 

    ![텍스트 및 색상 집합과 함께 레이블](launch-screens-images/launch08.png)

9. 오른쪽 단추를 사용 하 여 제약 조건을 편집 모드로 전환 된 **제약 조건을 도구 모음**:
    
    ![제약 조건 편집 모드 단추](launch-screens-images/launch09.png)

10. 제약 조건는 **이미지 보기**, 높이 너비를 설정 하 고 가로 및 세로로 가운데 맞춤 합니다.

    ![레이아웃 제약 조건으로는 이미지 보기](launch-screens-images/launch10.png)

    - 제약 조건을 추가 하는 방법에 대 한 자세한 내용은 참조 하십시오. [iOS 용 Xamarin 디자이너로 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md)합니다.

11. 제약 조건에서 **레이블**, 가로 가운데 맞춤, 높이 너비를 지정 및 고정 위치에서 세로로 거리는 **이미지 보기**:

    ![레이아웃 제약 조건 사용 하 여 레이블](launch-screens-images/launch11.png)

12. 다른 장치 및 모든 시나리오에서 의도 한 대로 디자인 표시 되는지 확인 하는 방향을 테스트 합니다. 특정 장치나 방향에 대 한 조정을 할 수 있는 경우 사용 하 여는 **특성 편집** 특정 크기 클래스에 대 한 제약을 추가 하려면 단추:

    ![시작 화면 가로 방향을 사용 하 여 X iPhone로 렌더링](launch-screens-images/launch12.png)

13. 스토리 보드의 변경 내용을 저장 합니다. 시뮬레이터 또는 장치에서 앱을 실행 하 고 응용 프로그램을 시작할 때 시작 화면이 표시 됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 새 프로젝트를 만듭니다. Visual Studio에서 선택 **파일 > 새로 만들기 > 프로젝트**를 선택한 후 **단일 보기 (iPhone) 앱**:
    
    ![사용 하 여 새 프로젝트 창, 단일 보기 (iPhone) 앱 선택](launch-screens-images/launch01-vs.png)

    - 프로젝트 이름을 위치를 선택 하 고 선택 **확인**합니다.

2. 경우 **리소스 > LaunchScreen.xib** 에 존재는 **솔루션 탐색기**, 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 삭제 **삭제**합니다. 이 파일은 다음 단계에서 스토리 보드로 바뀝니다.

3. 시작 화면으로 사용할 스토리 보드를 만듭니다. 에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 항목...**  이어서 **빈 스토리 보드**합니다. 이 스토리 보드의 이름을 **LaunchScreen.storyboard** 클릭 **추가**:

    ![빈 스토리 보드 선택 사용 하 여 새 항목 추가 창](launch-screens-images/launch03-vs.png)

4. 프로젝트를 사용 하 여 구성 **LaunchScreen.storyboard** 의 시작 화면 스토리 보드로:

    - 편집하기 위해 **솔루션 탐색기**에서 **Info.plist** 파일을 두 번 클릭하여 엽니다. 
    - 에 **시각적 자산** 탭, 설정 **시작 화면** 를 **LaunchScreen**합니다.

    ![Info.plist의 시작 화면 선택기](launch-screens-images/launch04-vs.png)

5. 시작 화면에서 사용 하기 위해 사용할 수 있도록 프로젝트의 자산 카탈로그에 이미지를 추가 합니다.

    - 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 **자산 카탈로그** 선택 **자산 카탈로그 추가**합니다. 이 새 자산 카탈로그의 이름을 **자산**:

    ![선택한 자산 카탈로그 사용 하 여 새 항목 추가 창](launch-screens-images/launch05-vs.png)

    - 로 새 이미지 설정 추가 **자산** 에 설명 된 대로 자산 카탈로그는 [는 자산 카탈로그 이미지를 설정 하는 이미지 추가](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 의 섹션은 [이미지 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 가이드입니다.

6. 열기 **LaunchScreen.storyboard** 에 두 번 클릭 하 여 편집을 위해는 **솔루션 탐색기**합니다.

    - 스토리 보드 파일을 편집 하려면 Visual Studio는 Mac 빌드 호스트에 연결 되어 있어야 합니다. 참조는 [Mac에 연결할](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 세부 정보에 대 한 가이드입니다.

7. 장치 및 방향을 시작 화면에서에서 스토리 보드 디자이너 iOS 미리 보기를 선택 합니다. 아래쪽 도구 모음에서 장치 선택 창을 열고 선택 **iPhone 4S** 및 **세로**: 
 
    ![장치 선택 도구 모음](launch-screens-images/launch07-vs.png)

    - 선택 하면 장치를 방향 iOS 디자이너 디자인을 미리 봅니다. 방법을 변경 합니다. 여기에서 선택 영역에 관계 없이 새로 추가 된 경우가 아니면 모든 장치와 방향을 제약 조건이 적용 됩니다는 **특성 편집** 별도로 지정 하는 데 사용 된 단추입니다. 

8. 추가 **뷰-컨트롤러** 에서 하나를 끌어서 스토리 보드에는 **도구 상자** 디자인 화면으로: 

    ![빈 뷰-컨트롤러 디자인 화면에 추가](launch-screens-images/launch08-vs.png)

9. 설정의 **배경** 보기 컨트롤러의 주 보기의 색입니다. 뷰-컨트롤러에서 중간에 클릭 하 여 보기를 선택 하 고 사용 하 여 배경 색 조정는 **속성 창**:
    
    ![자주색 배경색으로 단일 보기](launch-screens-images/launch09-vs.png)

10. 추가 **이미지 보기** 시작 화면으로 해당 소스를 설정 하 고 **이미지**:

    - 끌어서는 **이미지 보기** 에서 **도구 상자** 보기의 가운데에 있습니다.
    - 와 **이미지 보기** 가 선택 된 상태는 **위젯** 의 섹션은 **속성 창** 설정는 **이미지** 속성을 설정 하는 이미지 에 이미 추가 된 **자산** 자산 카탈로그입니다. 위치 및 크기는 **이미지 보기** 필요에 따라:
    
    ![이미지 속성 집합과 함께 이미지 보기는](launch-screens-images/launch10-vs.png)

11. 추가 **레이블** 아래는 **이미지 보기**:

    - 끌어서는 **레이블** 에서 **도구 상자** 디자인 화면으로 배치 아래는 **이미지 보기**합니다.
    - 특성에 대 한 설정의 **레이블** 를 사용 하는 **속성 창**:

    ![텍스트 및 색상 집합과 함께 레이블](launch-screens-images/launch11-vs.png) 

12. 오른쪽 단추를 사용 하 여 제약 조건을 편집 모드로 전환 된 **제약 조건을 도구 모음**:
    
    ![제약 조건 편집 모드 단추](launch-screens-images/launch12-vs.png) 

13. 제약 조건는 **이미지 보기**, 높이 너비를 설정 하 고 가로 및 세로로 가운데 맞춤 합니다.

    ![레이아웃 제약 조건으로는 이미지 보기](launch-screens-images/launch13-vs.png) 

    - 제약 조건을 추가 하는 방법에 대 한 정보를 참조 하십시오. [iOS 용 Xamarin 디자이너로 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md)합니다.

14. 제약 조건에서 **레이블**, 가로 가운데 맞춤, 높이 너비를 지정 및 고정 위치에서 세로로 거리는 **이미지 보기**:
    
    ![레이아웃 제약 조건 사용 하 여 레이블](launch-screens-images/launch14-vs.png) 

15. 다른 장치 및 모든 시나리오에서 의도 한 대로 디자인 표시 되는지 확인 하는 방향을 테스트 합니다. 특정 장치나 방향에 대 한 조정을 할 수 있는 경우 사용 하 여는 **특성 편집** 특정 크기 클래스에 대 한 제약을 추가 하려면 단추:

    ![시작 화면 가로 방향을 사용 하 여 X iPhone로 렌더링](launch-screens-images/launch15-vs.png) 

16. 스토리 보드의 변경 내용을 저장 합니다. 시뮬레이터 또는 장치에서 앱을 실행 하 고 응용 프로그램을 시작할 때 시작 화면이 표시 됩니다.

-----

> [!NOTE]
> 시작 화면으로 사용 되는 스토리 보드 _해야_ 만 간단 하 고 기본 제공 UI 요소를 포함 하 고 **없습니다** 모든 계산을 수행 하거나 사용자 지정 클래스에서 파생 합니다.

통합 스토리 보드를 사용 하 여 시작 화면을 만들기에 대 한 자세한 내용은 참조 하십시오는 [동적 시작 화면](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) 의 섹션은 [통합 스토리 보드](~/ios/user-interface/storyboards/unified-storyboards.md) 가이드입니다.

## <a name="migrating-to-launch-screen-storyboards"></a>시작 화면 스토리 보드 마이그레이션

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

해당 시작 화면에 대 한 스토리 보드를 사용 하는 기존 응용 프로그램을 업데이트할 때 마우스 오른쪽 단추로 클릭는 **프로젝트 이름** 에 **솔루션 탐색기** 선택 **추가**  >  **새 파일...** . 선택 **iOS** > **시작 화면** 클릭는 **새로** 단추:

![](launch-screens-images/storyboard02.png "시작 화면 iOS를 선택 합니다.")

다음으로 두 번 클릭는 `Info.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다. 아래 **시작 화면**, 위에서 만든 새 스토리 보드 파일을 선택 합니다.

![](launch-screens-images/storyboard09.png "위에서 만든 새 스토리 보드 파일 선택")


시작 화면으로 새 스토리 보드를 사용 하려면 다음을 수행 합니다.

1. 두 번 클릭는 `Info.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
2. 으로 스크롤하여는 **유니버설 시작 이미지** 열려 있는 편집기의 섹션에서 **시작 화면** 위에서 만든 스토리 보드의 이름 드롭다운을 선택: 

    ![](launch-screens-images/storyboard08.png "스토리 보드에 시작 화면 설정")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 프로젝트 이름을 마우스 오른쪽 단추로 클릭는 **솔루션 탐색기** 선택 **추가** > **새 파일...** : 

    ![](launch-screens-images/image012.png "새 파일 추가")
2. 시작 화면에 대 한 이름을 입력 하 고 클릭는 **추가** 단추: 

    ![](launch-screens-images/image013.png "시작 화면에 대 한 이름을 입력 합니다.")
3. 에 **솔루션 탐색기**, 새로 만든된 스토리 보드 파일을 편집 하기 위해 열을 두 번 클릭 합니다.
4. 확인는 **크기 클래스** 로 설정 되어 **모든: 모든** 및 **다른 이름으로 뷰** 은 **제네릭**: 

    ![](launch-screens-images/image016.png "크기 클래스에 설정 되어 있는지 확인: 모든 보기도 제네릭인 및")
5. 어셈블리 크기 클래스, 간단한 UI 요소에서에서 시작 화면 (예: `UIImageView`) 및 응용 프로그램의 번들에 포함 된 이미지: 

    ![](launch-screens-images/image017.png "어셈블리의 iOS 디자이너에서에서 실행 화면")
6. 스토리 보드의 변경 내용을 저장 합니다.

-----

## <a name="related-links"></a>관련 링크

- [동적 시작 화면 (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [통합 Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS Designer 기본 사항](~/ios/user-interface/designer/index.md)
- [자산 카탈로그 이미지에 추가 이미지 설정](~/ios/app-fundamentals/images-icons/displaying-an-image.md#asset-catalogs)
- [IOS 용 Xamarin 디자이너로 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md)
- [휴먼 인터페이스 지침: 실행 화면](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
