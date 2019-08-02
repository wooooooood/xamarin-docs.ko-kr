---
title: Xamarin에서 tvOS 텍스트 및 검색 필드 작업
description: 이 문서에서는 Xamarin으로 빌드된 tvOS 앱에서 텍스트 및 검색 필드를 사용 하는 방법을 설명 합니다. 텍스트 및 검색 필드에 대 한 개략적인 개요를 제공 하 고 키보드, 스토리 보드 통합, 검색 데이터 모델 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: cb8520a223a2bc10706c7e5bcebf8fc412d4e64e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652297"
---
# <a name="working-with-tvos-text-and-search-fields-in-xamarin"></a>Xamarin에서 tvOS 텍스트 및 검색 필드 작업

필요한 경우 tvOS 앱은 텍스트 필드 및 화상 키보드를 사용 하 여 사용자 (예: 사용자 Id 및 암호)의 작은 텍스트를 요청할 수 있습니다.

[![](text-fields-and-search-images/intro01.png "샘플 검색 필드")](text-fields-and-search-images/intro01.png#lightbox)

필요에 따라 검색 필드를 사용 하 여 앱 콘텐츠의 키워드 검색 기능을 제공할 수 있습니다.

[![](text-fields-and-search-images/intro02.png "샘플 검색 결과")](text-fields-and-search-images/intro02.png#lightbox)

이 문서에서는 tvOS 앱에서 텍스트 및 검색 필드 작업에 대 한 세부 정보를 다룹니다.

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>텍스트 및 검색 필드 정보

위에서 언급 한 것 처럼 필요한 경우 tvOS는 하나 이상의 텍스트 필드를 표시 하 여 화면 (또는 사용자가 설치한 tvOS 버전에 따라 선택적 블루투스 키보드)을 사용 하 여 사용자의 적은 양의 텍스트를 사용자에 게 수집할 수 있습니다. 

또한 앱이 사용자에 게 많은 양의 콘텐츠를 제공 하는 경우 (예: 음악, 동영상 또는 그림 컬렉션) 사용자가 적은 양의 텍스트를 입력 하 여 사용 가능한 항목의 목록을 필터링 할 수 있도록 하는 검색 필드를 포함할 수 있습니다.

<a name="Text-Fields" />

## <a name="text-fields"></a>텍스트 필드

TvOS에서 텍스트 필드는 사용자가 클릭 하면 화상 키보드를 표시 하는 고정 높이의 둥근 모퉁이 항목 상자로 표시 됩니다.

[![](text-fields-and-search-images/text01.png "TvOS의 텍스트 필드")](text-fields-and-search-images/text01.png#lightbox)

사용자가 지정 된 텍스트 필드에 [포커스](~/ios/tvos/app-fundamentals/navigation-focus.md) 를 이동 하면 더 크게 커지고 전체 그림자를 표시 합니다. 텍스트 필드는 포커스가 있을 때 다른 UI 요소와 겹칠 수 있으므로 사용자 인터페이스를 디자인할 때이 점을 염두에 두어야 합니다.

Apple에는 텍스트 필드 작업에 대 한 다음과 같은 제안이 있습니다.

- **텍스트 항목을 사용** 하지 않는 경우-화상 키보드의 특성상 긴 텍스트 섹션을 입력 하거나 여러 텍스트 필드를 채우는 것은 사용자에 게 지루한 일입니다. 더 나은 솔루션은 선택 목록 또는 [단추](~/ios/tvos/user-interface/buttons.md)를 사용 하 여 텍스트 입력의 양을 제한 하는 것입니다.
- **힌트를 사용 하 여 용도의 통신** -텍스트 필드가 비어 있는 경우 자리 표시자 "힌트"를 표시할 수 있습니다. 해당 하는 경우 힌트를 사용 하 여 별도의 레이블이 아닌 텍스트 필드의 용도를 설명 합니다.
- **적절 한 기본 키보드 종류를 선택** 합니다. TvOS은 텍스트 필드에 대해 지정할 수 있는 여러 가지 용도의 기본 키보드 유형을 제공 합니다. 예를 들어 전자 메일 주소 키보드는 사용자가 최근에 입력 한 주소 목록에서 선택할 수 있도록 하 여 쉽게 입력할 수 있습니다.
- **보안 텍스트 필드를 사용 하는 경우** 에는 보안 텍스트 필드를 사용 합니다. 보안 텍스트 필드는 실제 문자 대신 점으로 입력 된 문자를 표시 합니다. 암호와 같은 중요 한 정보를 수집 하는 경우 항상 보안 텍스트 필드를 사용 합니다.

<a name="Keyboards" />

## <a name="keyboards"></a>키보드

사용자가 사용자 인터페이스에서 텍스트 필드를 클릭할 때마다 선형 화상 키보드가 표시 됩니다. 사용자는 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) 터치 화면을 사용 하 여 키보드에서 개별 문자를 선택 하 고 요청 된 정보를 입력 합니다.

[![](text-fields-and-search-images/keyboard01.png "Siri 원격 키보드")](text-fields-and-search-images/keyboard01.png#lightbox)

현재 보기에 텍스트 필드가 두 개 이상 있으면 **다음 단추가 자동** 으로 표시 되어 사용자가 다음 텍스트 필드로 이동 합니다. 텍스트 입력을 종료 하 고 사용자를 이전 화면으로 반환 하는 마지막 텍스트 필드에 대해 **완료** 단추가 표시 됩니다. 

사용자는 언제 든 지 Siri Remote to end 텍스트 항목의 **메뉴** 단추를 누르고 다시 이전 화면으로 돌아갈 수 있습니다.

Apple은 화상 키보드를 사용할 때 다음과 같은 제안 사항을 제안 합니다.

- **적절 한 기본 키보드 종류를 선택** 합니다. TvOS은 텍스트 필드에 대해 지정할 수 있는 여러 가지 용도의 기본 키보드 유형을 제공 합니다. 예를 들어 전자 메일 주소 키보드는 사용자가 최근에 입력 한 주소 목록에서 선택할 수 있도록 하 여 쉽게 입력할 수 있습니다.
- **적절 한 경우 키보드 액세서리 보기 사용** -항상 표시 되는 표준 정보 외에, 이미지 또는 레이블과 같은 선택적 액세서리 보기를 화상 키보드에 추가 하 여 텍스트 입력의 용도를 명확 하 게 하거나 지원할 수 있습니다. 필요한 정보를 입력 하는 사용자입니다.

화상 키보드를 사용 하는 방법에 대 한 자세한 내용은 Apple의 [UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType), [키보드 관리](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [데이터 입력에 대 한 사용자 지정 보기](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) 및 [IOS 설명서에 대 한 텍스트 프로그래밍 가이드](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) 를 참조 하세요.

<a name="Search" />

## <a name="search"></a>검색

검색 필드는 사용자가 키보드 아래에 표시 되는 항목의 컬렉션을 필터링 할 수 있는 텍스트 필드 및 화상 키보드를 제공 하는 특수 한 화면을 제공 합니다.

[![](text-fields-and-search-images/search01.png "샘플 검색 결과")](text-fields-and-search-images/search01.png#lightbox)

사용자가 검색 필드에 문자를 입력 하면 아래 결과에 검색 결과가 자동으로 반영 됩니다. 사용자는 언제 든 지 결과에 포커스를 이동 하 고 표시 되는 항목 중 하나를 선택할 수 있습니다.

Apple에는 검색 필드 작업에 대 한 다음과 같은 제안이 있습니다.

- **최근 검색 제공** -Siri 원격으로 텍스트를 입력 하는 것은 지루한 일 수 있고 사용자가 검색 요청을 반복 하는 경향이 있기 때문에 키보드 영역에서 현재 결과 앞에 최근 검색 결과의 섹션을 추가 하는 것이 좋습니다.
- **가능 하면 결과 수를 제한** 합니다. 사용자가 구문 분석 하 여 탐색 하기 어려울 수 있는 많은 항목 목록이 있으므로 반환 된 결과 수를 제한 하는 것이 어려울 수 있습니다.
- **해당 하는 경우 검색 결과 필터 제공** -앱에서 제공 하는 콘텐츠가 자동으로 제공 되는 경우 사용자가 반환 되는 검색 결과를 추가로 필터링 할 수 있도록 범위 막대를 추가 하는 것이 좋습니다.

자세한 내용은 Apple의 [Uisearchcontroller 클래스 참조](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html)를 참조 하세요.

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>텍스트 필드 작업

TvOS 앱에서 텍스트 필드를 사용 하는 가장 쉬운 방법은 iOS Designer를 사용 하 여 사용자 인터페이스 디자인에 추가 하는 것입니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 편집용으로 엽니다.
1. 디자인 화면에서 하나 이상의 **텍스트 필드** 를 뷰로 끌어 옵니다. 

    [![](text-fields-and-search-images/text02.png "텍스트 필드")](text-fields-and-search-images/text02.png#lightbox)
1. **텍스트 필드** 를 선택 하 고 **Properties Pad**의 **위젯** 탭에서 각 고유 **이름을** 지정 합니다. 

    [![](text-fields-and-search-images/text03.png "Properties Pad의 위젯 탭")](text-fields-and-search-images/text03.png#lightbox)
1. **텍스트 필드** 섹션에서 **자리 표시자** 힌트 및 기본값과 같은 요소를 정의할 수 **있습니다.** 

    [![](text-fields-and-search-images/text04.png "텍스트 필드 섹션")](text-fields-and-search-images/text04.png#lightbox)
1. 아래로 스크롤하여 **맞춤법 검사**, **대문자** 표시 및 기본 **키보드 종류**와 같은 속성을 정의 합니다. 

    [![](text-fields-and-search-images/text05.png "맞춤법 검사, 대문자 표시 및 기본 키보드 종류")](text-fields-and-search-images/text05.png#lightbox) 
1. 스토리 보드에 대 한 변경 내용을 저장 합니다.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. 편집하기 위해 **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭하여 엽니다.
1. 디자인 화면에서 하나 이상의 **텍스트 필드** 를 뷰로 끌어 옵니다. 

    [![](text-fields-and-search-images/text02-vs.png "텍스트 필드")](text-fields-and-search-images/text02-vs.png#lightbox)
1. **텍스트 필드** 를 선택 하 고 **속성 탐색기**의 **위젯** 탭에서 각각 고유한 **이름을** 지정 합니다. 

    [![](text-fields-and-search-images/text03-vs.png "위젯 탭")](text-fields-and-search-images/text03-vs.png#lightbox)
1. **텍스트 필드** 섹션에서 **자리 표시자** 힌트 및 기본값과 같은 요소를 정의할 수 **있습니다.** 

    [![](text-fields-and-search-images/text04-vs.png "텍스트 필드 섹션")](text-fields-and-search-images/text04-vs.png#lightbox)
1. 아래로 스크롤하여 **맞춤법 검사**, **대문자** 표시 및 기본 **키보드 종류**와 같은 속성을 정의 합니다. 

    [![](text-fields-and-search-images/text05-vs.png "맞춤법 검사, 대문자 표시 및 기본 키보드 종류")](text-fields-and-search-images/text05-vs.png#lightbox) 
1. 스토리 보드에 대 한 변경 내용을 저장 합니다.
    
-----

코드에서 `Text` 속성을 사용 하 여 텍스트 필드의 값을 가져오거나 설정할 수 있습니다.

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

필요에 따라 `Started` 및 `Ended` 텍스트 필드 이벤트를 사용 하 여 시작 및 종료 텍스트 입력에 응답할 수 있습니다.

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>검색 필드 작업

TvOS 앱에서 검색 필드에 대 한 작업을 수행 하는 가장 쉬운 방법은 인터페이스 디자이너를 사용 하 여 사용자 인터페이스 디자인에 추가 하는 것입니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
1. **Solution Pad**에서 `Main.storyboard` 파일을 두 번 클릭 하 여 편집용으로 엽니다.
1. 새 컬렉션 뷰 컨트롤러를 스토리 보드로 끌어 사용자의 검색 결과를 표시 합니다. 

    [![](text-fields-and-search-images/search02.png "컬렉션 뷰 컨트롤러")](text-fields-and-search-images/search02.png#lightbox)
1. **Properties Pad** `SearchResultsViewController` 의 **위젯** 탭에서 **클래스** 및 `SearchResults` **스토리 보드 ID**에 대해를 사용 합니다. 

    [![](text-fields-and-search-images/search03.png "위젯 탭")](text-fields-and-search-images/search03.png#lightbox)
1. 디자인 화면에서 **셀 프로토타입을** 선택 합니다.
1. **속성 탐색기**의 **위젯** 탭에서 **클래스** 및 `ImageCell` **식별자**에 `SearchResultCell` 대해를 사용 합니다. 

    [![](text-fields-and-search-images/search04.png "위젯 탭")](text-fields-and-search-images/search04.png#lightbox)
1. **셀 프로토타입의** 디자인을 레이아웃 하 고 **속성 탐색기**의 **위젯** 탭에서 각 요소에 고유한 **이름을** 표시 합니다. 

    [![](text-fields-and-search-images/search05.png "셀 프로토타입의 디자인 레이아웃")](text-fields-and-search-images/search05.png#lightbox)
1. 스토리 보드에 대 한 변경 내용을 저장 합니다.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. 편집하기 위해 **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭하여 엽니다.
1. 새 컬렉션 뷰 컨트롤러를 스토리 보드로 끌어 사용자의 검색 결과를 표시 합니다. 

    [![](text-fields-and-search-images/seach02-vs.png "컬렉션 뷰 컨트롤러")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. **속성 탐색기**의 `SearchResultsViewController` **위젯** 탭에서 **클래스** 및 `SearchResults` **Storyboard ID**에 대해를 사용 합니다. 

    [![](text-fields-and-search-images/search03-vs.png "위젯 탭")](text-fields-and-search-images/search03-vs.png#lightbox)
1. 디자인 화면에서 **셀 프로토타입을** 선택 합니다.
1. **속성 탐색기**의 **위젯** 탭에서 **클래스** 및 `ImageCell` **식별자**에 `SearchResultCell` 대해를 사용 합니다. 

    [![](text-fields-and-search-images/search04-vs.png "위젯 탭")](text-fields-and-search-images/search04-vs.png#lightbox)
1. **셀 프로토타입의** 디자인을 레이아웃 하 고 **속성 탐색기**의 **위젯** 탭에서 각 요소에 고유한 **이름을** 표시 합니다. 

    [![](text-fields-and-search-images/search05-vs.png "셀 프로토타입의 디자인 레이아웃")](text-fields-and-search-images/search05-vs.png#lightbox)
1. 스토리 보드에 대 한 변경 내용을 저장 합니다.
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>데이터 모델 제공

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

다음으로, 사용자가 검색 하는 결과에 대 한 데이터 모델 역할을 할 클래스를 제공 해야 합니다. **솔루션 탐색기**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.일반빈클래스이며이름을제공합니다 > .  >  

[![](text-fields-and-search-images/search06.png "빈 클래스를 선택 하 고 이름을 입력 합니다.")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

다음으로, 사용자가 검색 하는 결과에 대 한 데이터 모델 역할을 할 클래스를 제공 해야 합니다. **솔루션 탐색기**에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 항목** ...을 선택 합니다.Apple기타 > 클래스 및이름을제공합니다. >   >  

[![](text-fields-and-search-images/search06-vs.png "클래스를 선택 하 고 이름을 입력 합니다.")](text-fields-and-search-images/search06-vs.png#lightbox)

-----

예를 들어 사용자가 제목 및 키워드를 기준으로 그림 컬렉션을 검색할 수 있도록 허용 하는 앱은 다음과 같습니다.

```csharp
using System;
using Foundation;

namespace tvText
{
    public class PictureInformation : NSObject
    {
        #region Computed Properties
        public string Title { get; set;}
        public string ImageName { get; set;}
        public string Keywords { get; set;}
        #endregion

        #region Constructors
        public PictureInformation (string title, string imageName, string keywords)
        {
            // Initialize
            this.Title = title;
            this.ImageName = imageName;
            this.Keywords = keywords;
        }
        #endregion
    }
}
```

<a name="The-Collection-View-Cell" />

### <a name="the-collection-view-cell"></a>컬렉션 뷰 셀

데이터 모델을 배치한 상태에서 **프로토타입 셀** (`SearchResultViewCell.cs`)을 편집 하 고 다음과 같이 표시 되도록 합니다.

```csharp
using Foundation;
using System;
using UIKit;

namespace tvText
{
    public partial class SearchResultViewCell : UICollectionViewCell
    {
        #region Private Variables
        private PictureInformation _pictureInfo = null;
        #endregion

        #region Computed Properties
        public PictureInformation PictureInfo {
            get { return _pictureInfo; }
            set {
                _pictureInfo = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            UpdateUI ();
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Anything to process?
            if (PictureInfo == null) return;

            try {
                Picture.Image = UIImage.FromBundle (PictureInfo.ImageName);
                Picture.AdjustsImageWhenAncestorFocused = true;
                Title.Text = PictureInfo.Title;
                TextColor = UIColor.LightGray;
            } catch {
                // Ignore errors if view isn't fully loaded
            }
        }
        #endregion
    }

}
```

메서드 `UpdateUI` 는 속성이 업데이트 될 때마다 명명 된 UI 요소에 있는 `PictureInfo` 속성의 개별 필드 (속성)를 표시 하는 데 사용 됩니다. 예를 들어 그림과 연결 된 이미지와 제목이 있습니다.

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>컬렉션 뷰 컨트롤러

그런 다음 검색 결과 컬렉션 뷰 컨트롤러 (`SearchResultsViewController.cs`)를 편집 하 고 다음과 같이 만듭니다.

```csharp
using Foundation;
using System;
using UIKit;
using System.Collections.Generic;

namespace tvText
{
    public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
    {
        #region Constants
        public const string CellID = "ImageCell";
        #endregion

        #region Private Variables
        private string _searchFilter = "";
        #endregion

        #region Computed Properties
        public List<PictureInformation> AllPictures { get; set;}
        public List<PictureInformation> FoundPictures { get; set; }
        public string SearchFilter {
            get { return _searchFilter; }
            set {
                _searchFilter = value.ToLower();
                FindPictures ();
                CollectionView?.ReloadData ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultsViewController (IntPtr handle) : base (handle)
        {
            // Initialize
            this.AllPictures = new List<PictureInformation> ();
            this.FoundPictures = new List<PictureInformation> ();
            PopulatePictures ();
            FindPictures ();

        }
        #endregion

        #region Private Methods
        private void PopulatePictures ()
        {
            // Clear list
            AllPictures.Clear ();

            // Add images
            AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
            AllPictures.Add (new PictureInformation ("Cheese Plate", "CheesePlate", "cheese,plate,bread"));
            AllPictures.Add (new PictureInformation ("Coffee House", "CoffeeHouse", "coffee,people,menu,restaurant,cafe"));
            AllPictures.Add (new PictureInformation ("Computer and Expresso", "ComputerExpresso", "computer,coffee,expresso,phone,notebook"));
            AllPictures.Add (new PictureInformation ("Hamburger", "Hamburger", "meat,bread,cheese,tomato,pickle,lettus"));
            AllPictures.Add (new PictureInformation ("Lasagna Dinner", "Lasagna", "salad,bread,plate,lasagna,pasta"));
            AllPictures.Add (new PictureInformation ("Expresso Meeting", "PeopleExpresso", "people,bag,phone,expresso,coffee,table,tablet,notebook"));
            AllPictures.Add (new PictureInformation ("Soup and Sandwich", "SoupAndSandwich", "soup,sandwich,bread,meat,plate,tomato,lettus,egg"));
            AllPictures.Add (new PictureInformation ("Morning Coffee", "TabletCoffee", "tablet,person,man,coffee,magazine,table"));
            AllPictures.Add (new PictureInformation ("Evening Coffee", "TabletMagCoffee", "tablet,magazine,coffee,table"));
        }

        private void FindPictures ()
        {
            // Clear list
            FoundPictures.Clear ();

            // Scan each picture for a match
            foreach (PictureInformation picture in AllPictures) {
                if (SearchFilter == "") {
                    // If no search term, everything matches
                    FoundPictures.Add (picture);
                } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
                    // If the search term is in the title or keywords, we've found a match
                    FoundPictures.Add (picture);
                }
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            // Only one section in this collection
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            // Return the number of matching pictures
            return FoundPictures.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a new cell and return it
            var cell = collectionView.DequeueReusableCell (CellID, indexPath);
            return (UICollectionViewCell)cell;
        }

        public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
        {
            // Grab the cell
            var currentCell = cell as SearchResultViewCell;
            if (currentCell == null)
                throw new Exception ("Expected to display a `SearchResultViewCell`.");

            // Display the current picture info in the cell
            var item = FoundPictures [indexPath.Row];
            currentCell.PictureInfo = item;
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // If this Search Controller was presented as a modal view, close
            // it before continuing
            // DismissViewController (true, null);

            // Grab the picture being selected and report it
            var picture = FoundPictures [indexPath.Row];
            Console.WriteLine ("Selected: {0}", picture.Title);
        }

        public void UpdateSearchResultsForSearchController (UISearchController searchController)
        {
            // Save the search filter and update the Collection View
            SearchFilter = searchController.SearchBar.Text ?? string.Empty;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
            if (previousItem != null) {
                UIView.Animate (0.2, () => {
                    previousItem.TextColor = UIColor.LightGray;
                });
            }

            var nextItem = context.NextFocusedView as SearchResultViewCell;
            if (nextItem != null) {
                UIView.Animate (0.2, () => {
                    nextItem.TextColor = UIColor.Black;
                });
            }
        }
        #endregion
    }
}
```

`IUISearchResultsUpdating` 먼저 인터페이스를 클래스에 추가 하 여 사용자가 업데이트 중인 검색 컨트롤러 필터를 처리 합니다.

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

또한 상수는 나중에 컬렉션 컨트롤러에서 새 셀을 요청할 때 사용 되는 **프로토타입 셀** 의 id (위의 인터페이스 디자이너에 정의 된 id와 일치)를 지정 하기 위해 정의 됩니다.

```csharp
public const string CellID = "ImageCell";
```

검색 되는 항목의 전체 목록, 검색 필터 용어 및 해당 용어와 일치 하는 항목 목록에 대 한 저장소가 만들어집니다.

```csharp
private string _searchFilter = "";
...

public List<PictureInformation> AllPictures { get; set;}
public List<PictureInformation> FoundPictures { get; set; }
public string SearchFilter {
    get { return _searchFilter; }
    set {
        _searchFilter = value.ToLower();
        FindPictures ();
        CollectionView?.ReloadData ();
    }
}
```

`SearchFilter` 이 변경 되 면 일치 하는 항목 목록이 업데이트 되 고 컬렉션 뷰의 콘텐츠가 다시 로드 됩니다. 루틴 `FindPictures` 은 새 검색 용어와 일치 하는 항목을 찾는 작업을 담당 합니다.

```csharp
private void FindPictures ()
{
    // Clear list
    FoundPictures.Clear ();

    // Scan each picture for a match
    foreach (PictureInformation picture in AllPictures) {
        if (SearchFilter == "") {
            // If no search term, everything matches
            FoundPictures.Add (picture);
        } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
            // If the search term is in the title or keywords, we've found a match
            FoundPictures.Add (picture);
        }
    }
}
```

사용자가 검색 컨트롤러 `SearchFilter` 에서 필터를 변경 하면의 값이 업데이트 됩니다 (결과 컬렉션 뷰를 업데이트 함).

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

메서드 `PopulatePictures` 는 처음에 사용 가능한 항목의 컬렉션을 채웁니다.

```csharp
private void PopulatePictures ()
{
    // Clear list
    AllPictures.Clear ();

    // Add images
    AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
    ...
}
```

이 예제에서는 컬렉션 뷰 컨트롤러를 로드할 때 모든 샘플 데이터가 메모리에 생성 됩니다. 실제 응용 프로그램에서는이 데이터를 데이터베이스 또는 웹 서비스에서 읽을 수 있으며, overrunning에서 Apple TV의 제한 된 메모리를 유지 하는 데 필요한 만큼만 데이터를 읽을 수 있습니다.

`NumberOfSections` 및`GetItemsCount` 메서드는 일치 하는 항목 수를 제공 합니다.

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    // Only one section in this collection
    return 1;
}

public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    // Return the number of matching pictures
    return FoundPictures.Count;
}
```

메서드 `GetCell` 는 컬렉션 뷰의 각 항목에 대 한 새 **프로토타입 셀** (Storyboard에서 위에 정의 된 `CellID` 항목 기반)을 반환 합니다.

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

`WillDisplayCell` 메서드는 구성 될 수 있도록 셀이 표시 되기 전에 호출 됩니다.

```csharp
public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
{
    // Grab the cell
    var currentCell = cell as SearchResultViewCell;
    if (currentCell == null)
        throw new Exception ("Expected to display a `SearchResultViewCell`.");

    // Display the current picture info in the cell
    var item = FoundPictures [indexPath.Row];
    currentCell.PictureInfo = item;
}
```

메서드 `DidUpdateFocus` 는 결과 컬렉션 뷰의 항목을 강조 표시 하는 사용자에 게 시각적 피드백을 제공 합니다.

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
    if (previousItem != null) {
        UIView.Animate (0.2, () => {
            previousItem.TextColor = UIColor.LightGray;
        });
    }

    var nextItem = context.NextFocusedView as SearchResultViewCell;
    if (nextItem != null) {
        UIView.Animate (0.2, () => {
            nextItem.TextColor = UIColor.Black;
        });
    }
}
```

마지막으로, `ItemSelected` 메서드는 결과 컬렉션 보기에서 항목을 선택 하는 사용자를 처리 합니다 (siri 원격으로 터치 서피스 클릭).

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // If this Search Controller was presented as a modal view, close
    // it before continuing
    // DismissViewController (true, null);

    // Grab the picture being selected and report it
    var picture = FoundPictures [indexPath.Row];
    Console.WriteLine ("Selected: {0}", picture.Title);
}
```

검색 필드가 모달 대화 상자 보기로 표시 된 경우 (이를 호출 하는 뷰의 위쪽에 있는) 메서드를 `DismissViewController` 사용 하 여 사용자가 항목을 선택할 때 검색 뷰를 해제 합니다. 이 예에서는 검색 필드가 탭 보기 탭의 콘텐츠로 표시 되므로 여기서는 해제 되지 않습니다.

컬렉션 뷰에 대 한 자세한 내용은 [컬렉션 뷰 작업](~/ios/tvos/user-interface/collection-views.md) 설명서를 참조 하세요.

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>검색 필드 프레젠테이션

TvOS에서 사용자에 게 검색 필드 (및 연결 된 화면 키보드 및 검색 결과)를 표시 하는 두 가지 주요 방법이 있습니다. 

- **모달 대화 상자 보기** -검색 필드는 현재 뷰 및 뷰 컨트롤러에 전체 화면 모달 대화 상자 보기로 표시 될 수 있습니다. 일반적으로이 작업은 단추 또는 다른 UI 요소를 클릭 하는 사용자에 게 응답 하 여 수행 됩니다. 사용자가 검색 결과에서 항목을 선택 하면 대화 상자가 닫힙니다.
- **내용 보기** -검색 필드는 지정 된 보기의 직접적인 부분입니다. 예를 들어 탭 보기 컨트롤러의 검색 탭 내용으로 사용할 경우입니다.

위에 제공 된 검색 가능한 그림 목록의 예를 보려면 검색 탭에서 검색 필드는 보기 내용으로 표시 되 고 검색 탭 보기 컨트롤러는 다음과 같습니다.

```csharp
using System;
using UIKit;

namespace tvText
{
    public partial class SecondViewController : UIViewController
    {
        #region Constants
        public const string SearchResultsID = "SearchResults";
        #endregion

        #region Computed Properties
        public SearchResultsViewController ResultsController { get; set;}
        #endregion

        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        public void ShowSearchController ()
        {
            // Build an instance of the Search Results View Controller from the Storyboard
            ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
            if (ResultsController == null)
                throw new Exception ("Unable to instantiate a SearchResultsViewController.");

            // Create an initialize a new search controller
            var searchController = new UISearchController (ResultsController) {
                SearchResultsUpdater = ResultsController,
                HidesNavigationBarDuringPresentation = false
            };

            // Set any required search parameters
            searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

            // The Search Results View Controller can be presented as a modal view
            // PresentViewController (searchController, true, null);

            // Or in the case of this sample, the Search View Controller is being
            // presented as the contents of the Search Tab directly. Use either one
            // or the other method to display the Search Controller (not both).
            var container = new UISearchContainerViewController (searchController);
            var navController = new UINavigationController (container);
            AddChildViewController (navController);
            View.Add (navController.View);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // If the Search Controller is being displayed as the content
            // of the search tab, include it here.
            ShowSearchController ();
        }

        public override void ViewDidAppear (bool animated)
        {
            base.ViewDidAppear (animated);

            // If the Search Controller is being presented as a modal view,
            // call it here to display it over the contents of the Search
            // tab.
            // ShowSearchController ();
        }
        #endregion
    }
}
```

먼저 인터페이스 디자이너에서 검색 결과 컬렉션 뷰 컨트롤러에 할당 된 **Storyboard 식별자** 와 일치 하는 상수가 정의 됩니다.

```csharp
public const string SearchResultsID = "SearchResults";
```

그런 다음, `ShowSearchController` 메서드는 새 검색 보기 컬렉션 컨트롤러를 만들어 필요한 것으로 표시 합니다.

```csharp
public void ShowSearchController ()
{
    // Build an instance of the Search Results View Controller from the Storyboard
    ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
    if (ResultsController == null)
        throw new Exception ("Unable to instantiate a SearchResultsViewController.");

    // Create an initialize a new search controller
    var searchController = new UISearchController (ResultsController) {
        SearchResultsUpdater = ResultsController,
        HidesNavigationBarDuringPresentation = false
    };

    // Set any required search parameters
    searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

    // The Search Results View Controller can be presented as a modal view
    // PresentViewController (searchController, true, null);

    // Or in the case of this sample, the Search View Controller is being
    // presented as the contents of the Search Tab directly. Use either one
    // or the other method to display the Search Controller (not both).
    var container = new UISearchContainerViewController (searchController);
    var navController = new UINavigationController (container);
    AddChildViewController (navController);
    View.Add (navController.View);
}
```

위의 메서드에서가 스토리 보드에서 인스턴스화된 `SearchResultsViewController` 후 검색 필드 및 화면 키보드를 사용자에 `UISearchController` 게 표시 하기 위해 새가 만들어집니다. 이 키보드 아래에는에 `SearchResultsViewController`정의 된 검색 결과 컬렉션이 표시 됩니다.

다음으로는 `SearchBar` **자리 표시자** 힌트와 같은 정보를 사용 하 여 구성 됩니다. 그러면 수행 중인 검색 유형에 대 한 정보를 사용자에 게 제공 합니다.

그런 다음 두 가지 방법 중 하나를 사용 하 여 검색 필드를 사용자에 게 표시 합니다.

- **모달 대화 상자 뷰** - `PresentViewController` 기존 뷰 전체 화면에 대 한 검색을 제공 하기 위해 메서드가 호출 됩니다.
- **콘텐츠 보기** -검색 `UISearchContainerViewController` 컨트롤러를 포함 하는를 만듭니다. 검색 컨테이너를 포함 하도록를 만든 다음 탐색 컨트롤러를 뷰 컨트롤러 `AddChildViewController (navController)`에 추가 하 고 보기 `View.Add (navController.View)`를 표시 합니다. `UINavigationController`

마지막으로, 그리고 프레젠테이션 유형에 `ViewDidLoad` 따라 또는 `ViewDidAppear` 메서드는 메서드를 `ShowSearchController` 호출 하 여 사용자에 게 검색을 제공 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // If the Search Controller is being displayed as the content
    // of the search tab, include it here.
    ShowSearchController ();
}

public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    // If the Search Controller is being presented as a modal view,
    // call it here to display it over the contents of the Search
    // tab.
    // ShowSearchController ();
}
```

앱이 실행 되 고 검색 탭이 사용자에 의해 선택 되 면 필터링 되지 않은 전체 항목 목록이 사용자에 게 표시 됩니다.

[![](text-fields-and-search-images/intro02.png "기본 검색 결과")](text-fields-and-search-images/intro02.png#lightbox)

사용자가 검색 용어를 입력 하기 시작 하면 결과 목록이 해당 용어를 기준으로 필터링 되 고 자동으로 업데이트 됩니다.

[![](text-fields-and-search-images/intro03.png "필터링 된 검색 결과")](text-fields-and-search-images/intro03.png#lightbox)

사용자는 언제 든 지 검색 결과의 항목으로 포커스를 전환 하 고 Siri 원격의 터치 화면을 클릭 하 여 선택할 수 있습니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱 내에서 텍스트 및 검색 필드를 디자인 하 고 작업 하는 방법에 대해 설명 했습니다. 인터페이스 디자이너에서 텍스트를 만들고 컬렉션 콘텐츠를 검색 하는 방법을 보여 주었습니다. tvOS에서 사용자에 게 검색 필드를 표시 하는 두 가지 다른 방법을 보여 주었습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
