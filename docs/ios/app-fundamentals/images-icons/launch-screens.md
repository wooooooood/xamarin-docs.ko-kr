---
title: Xamarin.iOS 앱에 대 한 시작 화면
description: 이 문서에서는 모든 해상도와 방향을 단일 통합 스토리 보드를 사용 하 여 모든 iOS 장치에서 앱 시작 화면을 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 31A489CA-756B-4B9B-B386-4BADF18EDD33
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/02/2018
ms.openlocfilehash: 40b8c38e89e96223bbf657ff06356d9fb2e9d9b3
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/10/2018
ms.locfileid: "40251187"
---
# <a name="launch-screens-for-xamarinios-apps"></a>Xamarin.iOS 앱에 대 한 시작 화면

_이 문서에서는 모든 해상도와 방향을 단일 통합 스토리 보드를 사용 하 여 모든 iOS 장치에서 앱 시작 화면을 만드는 방법을 설명 합니다._

IOS 8 하기 전에 iOS 앱에 대 한 시작 화면을 만드는 다양 한 장치 폼 팩터 및 앱을 실행할 수 있습니다 해상도 각각에 대해 이미지 자산을 제공 하는 개발자가 필요 합니다. 그러나 IOS 8의 릴리스 이후 받은 모든 경우에 올바르게 표시 하는 시작 화면을 만들려면 단일 통합 스토리 보드를 사용 하 여 합니다.

이 간략 한 연습 기존 프로젝트에 수동으로 추가 하는 스토리 보드 또는 새 프로젝트에는 기본적으로 제공 된 Storyboard를 사용 하 여 시작 화면을 만드는 방법을 설명 합니다. 그런 다음 iOS 디자이너를 사용 하 여 스토리 보드를 해당 뷰에 대 한 제약 조건을 설정 하 고 다양 한 장치 및 방향에 대 한 올바른 스토리 보드 표시 되는지 확인 하는 레이블 및 이미지 보기를 추가 하는 방법을 보여 줍니다.

<a name="storyboard" />

## <a name="managing-launch-screens-with-storyboards"></a>시작 화면 스토리 보드를 사용 하 여 관리

IOS 8 (이상)에서 개발자는 하나 이상의 정적 시작 이미지를 사용 하는 대신 시작 화면을 제공 하는 특수 통합 Storyboard를 만들 수 있습니다. IOS 디자이너에서에서 실행 하는 스토리 보드를 만들 때 여러 디스플레이 환경에 대 한 다른 레이아웃을 정의 하려면 Size 클래스 및 자동 레이아웃을 사용 합니다. 자동 레이아웃 및 Size 클래스를 사용 하 여 개발자는 모든 장치에서 정상적으로 진행 하는 단일 시작 화면을 만들 수 있으며 환경 표시.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac 용 Visual Studio에서 선택 하 여 새 프로젝트를 만듭니다 **파일 > 새 솔루션** 를 선택한 다음 **단일 뷰 앱**: 

    ![새 프로젝트 창 단일 뷰 앱 선택](launch-screens-images/launch01.png)

    - 기본적으로 새 프로젝트를 포함 한 **LaunchScreen.storyboard** 시작 화면 인터페이스를 정의 하는 파일입니다. 
    - 대신 시작 화면 스토리 보드에 추가할 기존 프로젝트를 마우스 오른쪽 단추로 클릭에서 프로젝트 이름을 합니다 **Solution Pad** 선택한 **추가 > 새 파일...**  선택한 후 **시작 화면**:

    ![새 파일 창 iOS 시작 화면 선택](launch-screens-images/launch01b.png)

    - 파일 이름을 **LaunchScreen** 또는 원하는 다른 이름입니다.

2. 시작 화면에 대 한 적절 한 스토리 보드를 사용 하도록 프로젝트를 구성 합니다.

    - 두 번 클릭 합니다 **Info.plist** 파일을 **Solution Pad** 을 편집용으로 엽니다.
    - 에 **시작 이미지** 섹션에서 확인 **시작 화면** 적절 한 스토리 보드의 이름으로 설정 됩니다.

    ![Info.plist에 있는 시작 화면 선택기](launch-screens-images/launch02.png)

    - 기본적으로 새 프로젝트를 사용 하도록 구성 됩니다 **LaunchScreen.storyboard** 해당 시작 화면으로 합니다.

3. 이미지를 추가 합니다 **Assets.xcassets** 자산 카탈로그 시작 화면에서 사용할 수 있도록 합니다. 자세한 내용은 참조 하세요. 합니다 [자산 카탈로그 이미지 집합에 이미지 추가](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 섹션을 [이미지를 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 가이드.

4. 오픈 **LaunchScreen.storyboard** 에서 두 번 클릭 하 여 편집을 위해 합니다 **Solution Pad**합니다.

5. 장치 및 iOS 디자이너에 있는 시작 화면 스토리 보드는 미리 보기 할 방향을 선택 합니다. 아래쪽 도구 모음에서 장치 선택 패널을 열고 선택 **iPhone 4 초** 하 고 **세로**합니다.

    ![장치 선택 도구 모음](launch-screens-images/launch05.png)

    - 선택 하면 장치 및 방향을 iOS 디자이너는 디자인을 미리 봅니다. 방법을 변경 합니다. 여기에 대 한 선택에 관계 없이 새로 추가 된 경우가 아니면 모든 장치 및 방향에서 제약 조건이 적용 됩니다는 **특성 편집** 다르게 지정 하는 데 사용 된 단추입니다. 

6. 설정 된 **백그라운드** 색 뷰 컨트롤러의 기본 보기입니다. 뷰 컨트롤러 중간 클릭 하 여 보기를 선택 하 고 사용 하 여 배경 색을 조정 합니다 **Properties Pad**:

    ![자주색 배경색을 사용 하 여 단일 보기](launch-screens-images/launch06.png)

7. 추가 **이미지 보기** 시작 화면에 해당 원본을 설정 하 고 **이미지**:

    - 끌어서를 **이미지 보기** 에서 합니다 **도구 상자 패드** 뷰의 가운데에 있습니다.
    - 사용 하 여는 **이미지 보기** 선택 된 경우를 **위젯** 부분을 **Properties Pad** 설정 합니다 **이미지** 속성을 이미 이미지 설정 에 추가 합니다 **Assets.xcassets** 자산 카탈로그입니다. 위치 및 크기를 **이미지 보기** 필요에 따라:
    
    ![이미지 속성 집합을 사용 하 여는 이미지 보기](launch-screens-images/launch07.png)

8. 추가 **레이블** 아래 합니다 **이미지 보기** 사용 하 여는 **Properties Pad** 해당 특성을 설정 하려면: 

    ![해당 텍스트 및 색 집합을 사용 하 여 레이블](launch-screens-images/launch08.png)

9. 오른쪽 단추를 사용 하 여 제약 조건을 편집 모드로 전환 합니다 **제약 조건 도구 모음**:
    
    ![제약 조건 편집 모드 단추](launch-screens-images/launch09.png)

10. 제약 조건을 추가 합니다 **이미지 보기**, 해당 높이 너비를 설정 하 고 가로 및 세로로 가운데:

    ![레이아웃 제약 조건으로는 이미지 보기](launch-screens-images/launch10.png)

    - 제약 조건을 추가 하는 방법에 대 한 자세한 내용은 참조 하세요. [iOS 용 Xamarin 디자이너를 사용 하 여 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md)합니다.

11. 제약 조건을 추가 합니다 **레이블**, 가로 가운데 맞춤, 높이 너비를 지정 하 고 고정 위치에서 세로 거리를 **이미지 보기**:

    ![레이아웃 제약 조건 레이블](launch-screens-images/launch11.png)

12. 다른 장치 및 화면 디자인 모든 시나리오에서 의도 한 대로 표시 되는지 확인을 테스트 합니다. 특정 장치 또는 방향에 대 한 조정 해야 하는 경우에 사용 된 **특성 편집** 제약 조건은 특정 크기 클래스를 추가 하려면 단추:

    ![IPhone X 가로 방향을 사용 하 여 렌더링 된 시작 화면](launch-screens-images/launch12.png)

13. 스토리 보드에 변경 내용을 저장 합니다. 시뮬레이터 또는 장치에서 앱을 실행 하 고 앱을 시작 하는 대로 시작 화면이 표시 됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 새 프로젝트를 만듭니다. Visual Studio에서 선택 **파일 > 새로 만들기 > 프로젝트 > Visual C# > iPhone 및 iPad > iOS 앱 (Xamarin)**:

    ![새 프로젝트 창 iOS 앱 (Xamarin) 선택](launch-screens-images/launch01.w157.png)

    선택 된 **단일 뷰 앱** 템플릿과 클릭 **확인**:

    ![단일 뷰 앱 템플릿](launch-screens-images/launch01-2.w157.png)

2. 하는 경우 **리소스 > LaunchScreen.xib** 에 존재 합니다 **솔루션 탐색기**, 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 삭제 **삭제**합니다. 이 파일은 다음 단계에서 스토리 보드로 바뀝니다.

3. 시작 화면으로 사용 하는 스토리 보드를 만듭니다. 에 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 항목...**  옵니다 **빈 스토리 보드**합니다. 이 스토리 보드의 이름을 **LaunchScreen.storyboard** 누릅니다 **추가**:

    ![새 항목 추가 창 빈 스토리 보드 선택](launch-screens-images/launch03.w157.png)

4. 사용 하도록 프로젝트 구성 **LaunchScreen.storyboard** 해당 시작 화면 스토리 보드로:

    - 편집하기 위해 **솔루션 탐색기**에서 **Info.plist** 파일을 두 번 클릭하여 엽니다. 
    - 에 **시각적 자산** 탭에서 **시작 화면** 하 **LaunchScreen**합니다.

    ![Info.plist에 있는 시작 화면 선택기](launch-screens-images/launch04-vs.png)

5. 시작 화면에서 사용할 수 있도록 프로젝트의 자산 카탈로그에 이미지를 추가 합니다.

    - 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 **자산 카탈로그** 선택한 **자산 카탈로그 추가**합니다. 이 새 자산 카탈로그의 이름을 **자산**:

    ![선택한 자산 카탈로그를 사용 하 여 새 항목 추가 창,](launch-screens-images/launch05.w157.png)

    - 에 새 이미지 집합을 추가 합니다 **자산** 에 설명 된 대로 자산 카탈로그를 [자산 카탈로그 이미지 집합에 이미지 추가](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 섹션을 [이미지를 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 가이드.

6. 오픈 **LaunchScreen.storyboard** 에서 두 번 클릭 하 여 편집을 위해 합니다 **솔루션 탐색기**합니다.

    - 스토리 보드 파일을 편집 하려면 Visual Studio를 Mac 빌드 호스트에 대 한 활성 연결을 해야 합니다. 참조 된 [Mac에 연결](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 자세한 가이드입니다.

7. 장치 및 iOS 디자이너에 있는 시작 화면 스토리 보드는 미리 보기 할 방향을 선택 합니다. 아래쪽 도구 모음에서 장치 선택 패널을 열고 선택 **iPhone 4 초** 하 고 **세로**: 
 
    ![장치 선택 도구 모음](launch-screens-images/launch07-vs.png)

    - 선택 하면 장치 및 방향을 iOS 디자이너는 디자인을 미리 봅니다. 방법을 변경 합니다. 여기에 대 한 선택에 관계 없이 새로 추가 된 경우가 아니면 모든 장치 및 방향에서 제약 조건이 적용 됩니다는 **특성 편집** 다르게 지정 하는 데 사용 된 단추입니다. 

8. 추가 **뷰 컨트롤러** 끌어에서 스토리 보드에는 **도구 상자** 디자인 화면으로: 

    ![디자인 화면에 빈 뷰 컨트롤러 추가](launch-screens-images/launch08-vs.png)

9. 설정 된 **백그라운드** 색 뷰 컨트롤러의 기본 보기입니다. 뷰 컨트롤러 중간 클릭 하 여 보기를 선택 하 고 사용 하 여 배경 색을 조정 합니다는 **속성 창**:
    
    ![자주색 배경색을 사용 하 여 단일 보기](launch-screens-images/launch09-vs.png)

10. 추가 **이미지 보기** 시작 화면에 해당 원본을 설정 하 고 **이미지**:

    - 끌어서를 **이미지 보기** 에서 합니다 **도구 상자** 뷰의 가운데에 있습니다.
    - 사용 하 여는 **이미지 보기** 선택 된 경우 여전히를 **위젯** 부분을 **속성 창** 설정 합니다 **이미지** 속성을 설정 하는 이미지 에 이미 추가 된 **자산** 자산 카탈로그입니다. 위치 및 크기를 **이미지 보기** 필요에 따라:
    
    ![이미지 속성 집합을 사용 하 여는 이미지 보기](launch-screens-images/launch10-vs.png)

11. 추가 된 **레이블** 아래 합니다 **보기 이미지**:

    - 끌어서를 **레이블을** 에서 **도구 상자** 디자인 화면으로 배치 아래를 **이미지 보기**합니다.
    - 특성을 설정 합니다 **레이블** 사용 하는 **속성 창**:

    ![해당 텍스트 및 색 집합을 사용 하 여 레이블](launch-screens-images/launch11-vs.png) 

12. 오른쪽 단추를 사용 하 여 제약 조건을 편집 모드로 전환 합니다 **제약 조건 도구 모음**:
    
    ![제약 조건 편집 모드 단추](launch-screens-images/launch12-vs.png) 

13. 제약 조건을 추가 합니다 **이미지 보기**, 해당 높이 너비를 설정 하 고 가로 및 세로로 가운데:

    ![레이아웃 제약 조건으로는 이미지 보기](launch-screens-images/launch13-vs.png) 

    - 제약 조건을 추가 하는 방법에 대 한 내용은 [iOS 용 Xamarin 디자이너를 사용 하 여 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md)합니다.

14. 제약 조건을 추가 합니다 **레이블**, 가로 가운데 맞춤, 높이 너비를 지정 하 고 고정 위치에서 세로 거리를 **이미지 보기**:
    
    ![레이아웃 제약 조건 레이블](launch-screens-images/launch14-vs.png) 

15. 다른 장치 및 화면 디자인 모든 시나리오에서 의도 한 대로 표시 되는지 확인을 테스트 합니다. 특정 장치 또는 방향에 대 한 조정 해야 하는 경우에 사용 된 **특성 편집** 제약 조건은 특정 크기 클래스를 추가 하려면 단추:

    ![IPhone X 가로 방향을 사용 하 여 렌더링 된 시작 화면](launch-screens-images/launch15-vs.png) 

16. 스토리 보드에 변경 내용을 저장 합니다. 시뮬레이터 또는 장치에서 앱을 실행 하 고 앱을 시작 하는 대로 시작 화면이 표시 됩니다.

-----

> [!NOTE]
> 시작 화면으로 사용 되는 스토리 보드 _해야 합니다_ 만 간단 하 고 기본 제공 UI 요소를 포함 및 **없습니다** 모든 계산을 수행 하거나 사용자 지정 클래스를 파생 합니다.

통합 스토리 보드를 사용 하 여 시작 화면을 만드는 방법에 대 한 자세한 내용은 참조 하십시오 합니다 [동적 시작 화면](~/ios/user-interface/storyboards/unified-storyboards.md#dynamic-launch-screens) 섹션을 [통합 스토리 보드](~/ios/user-interface/storyboards/unified-storyboards.md) 가이드입니다.

## <a name="migrating-to-launch-screen-storyboards"></a>시작 화면 스토리 보드 마이그레이션

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

해당 시작 화면에 대 한 스토리 보드를 사용 하도록 기존 앱을 업데이트할 때 마우스 오른쪽 단추로 클릭 합니다 **프로젝트 이름** 에 **솔루션 탐색기** 선택한 **추가**  >  **새 파일...** . 선택 **iOS** > **시작 화면** 을 클릭 합니다 **New** 단추:

![](launch-screens-images/storyboard02.png "IOS 시작 화면 선택")

를 두 번 클릭 합니다 `Info.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다. 아래 **시작 화면**, 위에서 만든 새 스토리 보드 파일을 선택 합니다.

![](launch-screens-images/storyboard09.png "위에서 만든 새 스토리 보드 파일을 선택")


시작 화면에서 새 스토리 보드를 사용 하려면 다음을 수행 합니다.

1. 두 번 클릭 합니다 `Info.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
2. 스크롤하여를 **유니버설 시작 이미지** 열린 편집기의 섹션을 **시작 화면** 스토리 보드의 이름을 위에서 만든 드롭다운을 선택: 

    ![](launch-screens-images/storyboard08.png "스토리 보드에 시작 화면 설정")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다 **솔루션 탐색기** 선택한 **추가** > **새 파일...** : 

    ![](launch-screens-images/image012.png "새 파일 추가")
2. 시작 화면에 대 한 이름을 입력 하 고 클릭 합니다 **추가** 단추: 

    ![](launch-screens-images/image013.png "시작 화면에 대 한 이름을 입력 합니다.")
3. 에 **솔루션 탐색기**, 새로 만든된 스토리 보드 파일을 편집 하는 것에 대 한 열을 두 번 클릭 합니다.
4. 있는지 확인 합니다 **Size 클래스** 로 설정 된 **모든: 모든** 및 **표시 방식** 은 **제네릭**: 

    ![](launch-screens-images/image016.png "Size 클래스에 설정 되어 있는지 확인 합니다: 모든 이며 보기와 일반")
5. 어셈블리 크기 클래스, 간단한 UI 요소에서에서 시작 화면 (같은 `UIImageView`) 및 응용 프로그램의 번들에 포함 된 이미지: 

    ![](launch-screens-images/image017.png "어셈블리의 시작 화면에서 iOS 디자이너")
6. 스토리 보드에 변경 내용을 저장 합니다.

-----

## <a name="related-links"></a>관련 링크

- [동적 시작 화면 (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [통합 Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md)
- [iOS Designer 기본 사항](~/ios/user-interface/designer/index.md)
- [자산 카탈로그 이미지에 추가 이미지 설정](~/ios/app-fundamentals/images-icons/displaying-an-image.md#adding-images-to-an-asset-catalog-image-set)
- [IOS 용 Xamarin 디자이너를 사용 하 여 자동 레이아웃](~/ios/user-interface/designer/designer-auto-layout.md)
- [휴먼 인터페이스 지침: 시작 화면](https://developer.apple.com/ios/human-interface-guidelines/icons-and-images/launch-screen/)
