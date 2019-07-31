---
title: Xamarin.ios 앱에 대 한 시작 화면
description: 이 문서에서는 단일 통합 Storyboard를 사용 하 여 모든 iOS 장치에 대 한 앱 시작 화면을 모든 해상도 및 방향으로 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 05/02/2018
ms.openlocfilehash: 76e9d91b735f2ae5041330d8e290347ae9314487
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68654813"
---
# <a name="launch-screens-for-xamarinios-apps"></a>Xamarin.ios 앱에 대 한 시작 화면

_이 문서에서는 단일 통합 Storyboard를 사용 하 여 모든 iOS 장치에 대 한 앱 시작 화면을 모든 해상도 및 방향으로 만드는 방법을 설명 합니다._

IOS 8 이전에는 iOS 앱에 대 한 시작 화면을 만들기 위해 개발자가 앱을 실행할 수 있는 다양 한 장치 폼 팩터 및 해상도 각각에 대해 이미지 자산을 제공 해야 했습니다. 그러나 iOS 8의 릴리스 이후에는 단일 통합 Storyboard를 사용 하 여 모든 경우에 올바른 모양의 시작 화면을 만들 수 있었습니다.

이 간단한 연습에서는 새 프로젝트에서 기본적으로 제공 되는 Storyboard 나 기존 프로젝트에 수동으로 추가 된 Storyboard를 사용 하 여 시작 화면을 만드는 방법을 설명 합니다. 그런 다음 iOS 디자이너를 사용 하 여 이미지 뷰 및 레이블을 스토리 보드에 추가 하 고, 해당 뷰에 대 한 제약 조건을 설정 하 고, 다양 한 장치 및 방향에 대 한 스토리 보드가 올바른지 확인 하는 방법을 보여 줍니다.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>Storyboard를 사용 하 여 시작 화면 관리

IOS 8 (이상)에서 개발자는 하나 이상의 정적 시작 이미지를 사용 하는 대신 특정 통합 Storyboard를 만들어 시작 화면을 제공할 수 있습니다. IOS 디자이너에서 시작 Storyboard를 만들 때 크기 클래스 및 자동 레이아웃을 사용 하 여 다양 한 표시 환경에 대해 다른 레이아웃을 정의 합니다. 개발자는 크기 클래스와 자동 레이아웃을 사용 하 여 모든 장치 및 디스플레이 환경에서 잘 보이는 단일 시작 화면을 만들 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac용 Visual Studio에서 **파일 > 새 솔루션** 을 선택한 다음 **단일 뷰 앱**을 선택 하 여 새 프로젝트를 만듭니다. 

    ![단일 뷰 앱이 선택 된 새 프로젝트 창](launch-screens-images/launch01.png)

    - 기본적으로 새 프로젝트에는 시작 화면 인터페이스를 정의 하는 **Launchscreen. storyboard** 파일이 포함 되어 있습니다. 
    - 대신 기존 프로젝트에 시작 화면 스토리 보드를 추가 하려면 **Solution Pad** 에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 파일** ...을 선택한 후 **시작 화면**을 선택 합니다.

    ![IOS 실행 화면이 선택 된 새 파일 창](launch-screens-images/launch01b.png)

    - File **Launchscreen** 의 이름을 선택 하거나 다른 이름을 선택 합니다.

2. 시작 화면에 적절 한 Storyboard를 사용 하도록 프로젝트를 구성 합니다.

    - **Solution Pad** 에서 info.plist 파일을 두 번 클릭 하 여 편집을 위해 엽니다 **.**
    - **시작 이미지** 섹션에서 **시작 화면** 이 적절 한 Storyboard의 이름으로 설정 되어 있는지 확인 합니다.

    ![Info.plist의 시작 화면 선택기](launch-screens-images/launch02.png)

    - 기본적으로 새 프로젝트는 실행 화면으로 **Launchscreen. storyboard** 를 사용 하도록 구성 됩니다.

3. **Assets.xcassets** 자산 카탈로그에 이미지를 추가 하 여 시작 화면에서 사용할 수 있도록 합니다. 자세한 내용은 [이미지 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 가이드의 [자산 카탈로그 이미지 집합에 이미지 추가](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 섹션을 참조 하세요.

4. **Solution Pad**에서 편집을 두 번 클릭 하 여 편집을 위해 **launchscreen** 을 엽니다.

5. IOS 디자이너에서 시작 화면 스토리 보드를 미리 볼 장치와 방향을 선택 합니다. 아래쪽 도구 모음에서 장치 선택 패널을 열고 **IPhone 4S** 및 **세로**를 선택 합니다.

    ![장치 선택 도구 모음](launch-screens-images/launch05.png)

    - 장치 및 방향 선택은 iOS 디자이너가 디자인을 미리 보는 방법만 변경 합니다. 여기에서 선택한 항목에 관계 없이 새로 추가 된 제약 조건은 모든 장치 및 방향에 적용 됩니다. 그렇지 않으면 **특성 편집** 단추를 사용 하 여 지정 된 경우 

6. 뷰 컨트롤러의 기본 보기에 대 한 **배경색** 을 설정 합니다. 뷰 컨트롤러의 가운데를 클릭 하 여 보기를 선택 하 고 **Properties Pad**를 사용 하 여 배경색을 조정 합니다.

    ![자주색 배경색을 사용 하는 단일 뷰입니다.](launch-screens-images/launch06.png)

7. 시작 화면에 **이미지 뷰** 를 추가 하 고 해당 원본 **이미지**를 설정 합니다.

    - **도구 상자 패드** 의 **이미지 뷰** 를 뷰의 가운데로 끌어 옵니다.
    - **이미지 뷰가** 선택 된 상태에서 **Properties Pad** 의 **위젯** 섹션에서 **이미지** 속성을 **assets.xcassets** Asset Catalog에 이미 추가 된 이미지 집합으로 설정 합니다. 필요에 따라 **이미지 보기** 의 위치를 변경 하 고 크기를 조정 합니다.
    
    ![이미지 속성이 설정 된 이미지 뷰](launch-screens-images/launch07.png)

8. **이미지 보기** 아래에 **레이블을** 추가 하 고 **Properties Pad** 를 사용 하 여 해당 특성을 설정 합니다. 

    ![텍스트와 색이 설정 된 레이블입니다.](launch-screens-images/launch08.png)

9. **제약 조건 도구 모음**에서 오른쪽 단추를 사용 하 여 제약 조건 편집 모드로 전환 합니다.
    
    ![제약 조건 편집 모드 단추](launch-screens-images/launch09.png)

10. **이미지 뷰에**제약 조건을 추가 하 고 높이와 너비를 설정 하 고 가로 및 세로로 가운데 맞춤 합니다.

    ![레이아웃 제약 조건이 있는 이미지 뷰](launch-screens-images/launch10.png)

    - 제약 조건을 추가 하는 방법에 대 한 자세한 내용은 [Xamarin Designer for iOS 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md)을 참조 하세요.

11. **레이블에**제약 조건을 추가 하 고, 가로로 가운데 맞춤 하 고, 높이와 너비를 제공 하 고, **이미지 뷰에서**세로 방향으로 고정 거리를 지정 합니다.

    ![레이아웃 제약 조건이 있는 레이블](launch-screens-images/launch11.png)

12. 다른 장치 및 방향을 테스트 하 여 디자인이 모든 시나리오에서 의도 한 대로 나타나는지 확인 합니다. 특정 장치나 방향에 대 한 조정을 수행 해야 하는 경우 **특성 편집** 단추를 사용 하 여 특정 크기 클래스에 대 한 제약 조건은를 추가 합니다.

    ![가로 방향을 사용 하 여 iPhone X로 렌더링 된 시작 화면](launch-screens-images/launch12.png)

13. 스토리 보드에 대 한 변경 내용을 저장 합니다. 시뮬레이터 또는 장치에서 앱을 실행 하면 앱이 시작 될 때 시작 화면이 표시 됩니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 새 프로젝트를 만듭니다. Visual Studio에서 **파일 > 새 > 프로젝트 > Visual C# > iPhone & IPad > IOS 앱 (Xamarin)** 을 선택 합니다.

    ![IOS 앱 (Xamarin)이 선택 된 새 프로젝트 창](launch-screens-images/launch01.w157.png)

    **단일 뷰 앱** 템플릿을 선택 하 고 **확인**을 클릭 합니다.

    ![단일 뷰 앱 템플릿](launch-screens-images/launch01-2.w157.png)

2. **리소스 > launchscreen** 가 **솔루션 탐색기**에 있으면 해당 파일을 마우스 오른쪽 단추로 클릭 하 고 **삭제**를 선택 하 여 삭제 합니다. 이 파일은 다음 단계에서 Storyboard로 바뀝니다.

3. 시작 화면으로 사용할 스토리 보드를 만듭니다. **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 항목** ...을 선택한 다음 **빈 스토리 보드**를 선택 합니다. 이 스토리 보드의 이름을 클릭 **하십시오. 스토리 보드** 를 클릭 하 고 **추가**를 클릭 합니다.

    ![빈 스토리 보드가 선택 된 새 항목 추가 창](launch-screens-images/launch03.w157.png)

4. 시작 화면 Storyboard로 **Launchscreen storyboard** 를 사용 하도록 프로젝트를 구성 합니다.

    - 편집하기 위해 **솔루션 탐색기**에서 **Info.plist** 파일을 두 번 클릭하여 엽니다. 
    - **시각적 자산** 탭에서 **시작 화면** 을 **launchscreen**로 설정 합니다.

    ![Info.plist의 시작 화면 선택기](launch-screens-images/launch04-vs.png)

5. 시작 화면에서 사용할 수 있도록 프로젝트의 자산 카탈로그에 이미지를 추가 합니다.

    - **솔루션 탐색기**에서 **자산 카탈로그** 를 마우스 오른쪽 단추로 클릭 하 고 **자산 카탈로그 추가**를 선택 합니다. 새 자산 카탈로그 **자산**이름:

    ![자산 카탈로그가 선택 된 새 항목 추가 창](launch-screens-images/launch05.w157.png)

    - [이미지 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 가이드의 [자산 카탈로그 이미지 집합에 이미지 추가](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 섹션에 설명 된 대로 **자산** 자산 카탈로그에 새 이미지 집합을 추가 합니다.

6. **솔루션 탐색기**에서 편집을 두 번 클릭 하 여 편집을 위해 **launchscreen** 을 엽니다.

    - 스토리 보드 파일을 편집 하려면 Visual Studio에서 Mac 빌드 호스트에 대 한 활성 연결이 필요 합니다. 자세한 내용은 [Mac에 연결](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 가이드를 참조 하세요.

7. IOS 디자이너에서 시작 화면 스토리 보드를 미리 볼 장치와 방향을 선택 합니다. 아래쪽 도구 모음에서 장치 선택 패널을 열고 **IPhone 4S** 및 **세로**를 선택 합니다. 
 
    ![장치 선택 도구 모음](launch-screens-images/launch07-vs.png)

    - 장치 및 방향 선택은 iOS 디자이너가 디자인을 미리 보는 방법만 변경 합니다. 여기에서 선택한 항목에 관계 없이 새로 추가 된 제약 조건은 모든 장치 및 방향에 적용 됩니다. 그렇지 않으면 **특성 편집** 단추를 사용 하 여 지정 된 경우 

8. **도구 상자** 에서 디자인 화면으로 뷰 컨트롤러를 끌어서 스토리 보드에 **뷰 컨트롤러** 를 추가 합니다. 

    ![디자인 화면에 추가 된 빈 뷰 컨트롤러](launch-screens-images/launch08-vs.png)

9. 뷰 컨트롤러의 기본 보기에 대 한 **배경색** 을 설정 합니다. 뷰 컨트롤러의 가운데를 클릭 하 여 보기를 선택 하 고 **속성 창을**사용 하 여 배경색을 조정 합니다.
    
    ![자주색 배경색을 사용 하는 단일 뷰입니다.](launch-screens-images/launch09-vs.png)

10. 시작 화면에 **이미지 뷰** 를 추가 하 고 해당 원본 **이미지**를 설정 합니다.

    - **도구 상자** 의 **이미지 뷰** 를 뷰의 가운데로 끌어 옵니다.
    - **이미지 뷰가** 선택 된 상태에서 **속성 창의** **위젯** 섹션에서 **이미지** 속성을 **자산** 자산 카탈로그에 이미 추가 된 이미지 집합으로 설정 합니다. 필요에 따라 **이미지 보기** 의 위치를 변경 하 고 크기를 조정 합니다.
    
    ![이미지 속성이 설정 된 이미지 뷰](launch-screens-images/launch10-vs.png)

11. **이미지 보기**아래에 **레이블을** 추가 합니다.

    - **도구 상자** 에서 디자인 화면으로 **레이블을** 끌어 **이미지 뷰**아래에 배치 합니다.
    - **속성 창을**사용 하 여 **레이블에** 대 한 특성을 설정 합니다.

    ![텍스트와 색이 설정 된 레이블입니다.](launch-screens-images/launch11-vs.png) 

12. **제약 조건 도구 모음**에서 오른쪽 단추를 사용 하 여 제약 조건 편집 모드로 전환 합니다.
    
    ![제약 조건 편집 모드 단추](launch-screens-images/launch12-vs.png) 

13. **이미지 뷰에**제약 조건을 추가 하 고 높이와 너비를 설정 하 고 가로 및 세로로 가운데 맞춤 합니다.

    ![레이아웃 제약 조건이 있는 이미지 뷰](launch-screens-images/launch13-vs.png) 

    - 제약 조건을 추가 하는 방법에 대 한 자세한 내용은 [Xamarin Designer for iOS 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md)을 참조 하세요.

14. **레이블에**제약 조건을 추가 하 고, 가로로 가운데 맞춤 하 고, 높이와 너비를 제공 하 고, **이미지 뷰에서**세로 방향으로 고정 거리를 지정 합니다.
    
    ![레이아웃 제약 조건이 있는 레이블](launch-screens-images/launch14-vs.png) 

15. 다른 장치 및 방향을 테스트 하 여 디자인이 모든 시나리오에서 의도 한 대로 나타나는지 확인 합니다. 특정 장치나 방향에 대 한 조정을 수행 해야 하는 경우 **특성 편집** 단추를 사용 하 여 특정 크기 클래스에 대 한 제약 조건은를 추가 합니다.

    ![가로 방향을 사용 하 여 iPhone X로 렌더링 된 시작 화면](launch-screens-images/launch15-vs.png) 

16. 스토리 보드에 대 한 변경 내용을 저장 합니다. 시뮬레이터 또는 장치에서 앱을 실행 하면 앱이 시작 될 때 시작 화면이 표시 됩니다.

-----

> [!NOTE]
> 시작 화면으로 사용 되는 스토리 보드에는 간단한 기본 제공 UI 요소만 포함 _되어야_ 하며 계산을 수행 하거나 사용자 지정 클래스에서 파생할 **수 없습니다** .

통합 Storyboard를 사용 하 여 시작 화면을 만드는 방법에 대 한 자세한 내용은 [통합 된 스토리 보드](~/ios/user-interface/storyboards/unified-storyboards.md) 가이드의 [동적 시작 화면](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) 섹션을 참조 하십시오.

## <a name="migrating-to-launch-screen-storyboards"></a>시작 화면 Storyboard로 마이그레이션

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

시작 화면에 storyboard를 사용 하도록 기존 앱을 업데이트 하는 경우 **솔루션 탐색기** 에서 **프로젝트 이름을** 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. **IOS** > **시작 화면** 을 선택 하 고 **새로 만들기** 단추를 클릭 합니다.

![](launch-screens-images/storyboard02.png "IOS 시작 화면을 선택 합니다.")

그런 다음 `Info.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다. **시작 화면**에서 위에서 만든 새 스토리 보드 파일을 선택 합니다.

![](launch-screens-images/storyboard09.png "위에서 만든 새 스토리 보드 파일을 선택 합니다.")


새 스토리 보드를 시작 화면으로 사용 하려면 다음을 수행 합니다.

1. `Info.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. 편집기의 **유니버설 시작 이미지** 섹션으로 스크롤하고 **시작 화면** 드롭다운을 열고 위에서 만든 storyboard의 이름을 선택 합니다. 

    ![](launch-screens-images/storyboard08.png "시작 화면을 storyboard로 설정")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기** 에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다. 

    ![](launch-screens-images/image012.png "새 파일 추가")
2. 시작 화면에 대 한 이름을 입력 하 고 **추가** 단추를 클릭 합니다. 

    ![](launch-screens-images/image013.png "시작 화면에 대 한 이름 입력")
3. **솔루션 탐색기**에서 새로 만든 스토리 보드 파일을 두 번 클릭 하 여 편집용으로 엽니다.
4. **Size 클래스** 를 any로 설정 **하 고** 뷰를 **제네릭** **로** 설정 합니다. 

    ![](launch-screens-images/image016.png "Size 클래스가 any로 설정 되었는지 확인 하 고 뷰를 제네릭로 설정 합니다.")
5. 응용 프로그램의 번들에 포함 된 크기 클래스, 간단한 UI 요소 ( `UIImageView`예:) 및 이미지에서 시작 화면을 어셈블리 합니다. 

    ![](launch-screens-images/image017.png "어셈블리 iOS 디자이너의 시작 화면")
6. 스토리 보드에 대 한 변경 내용을 저장 합니다.

-----

## <a name="related-links"></a>관련 링크

- [동적 실행 화면 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-dynamiclaunchscreen)
- [통합 Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS Designer 기본 사항](~/ios/user-interface/designer/index.md)
- [자산 카탈로그 이미지 집합에 이미지 추가](~/ios/app-fundamentals/images-icons/displaying-an-image.md#adding-images-to-an-asset-catalog-image-set)
- [Xamarin Designer for iOS 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md)
- [인적 인터페이스 지침: 시작 화면](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
