---
title: TvOS 텍스트 및 Xamarin에서 검색 필드를 사용 하 여 작업
description: 이 문서에서는 Xamarin에 내장 된 tvOS 앱에서 텍스트 및 검색 필드를 사용 하는 방법을 설명 합니다. 텍스트 및 검색 필드의 개략적인 개요를 제공 하 고 키보드, 스토리 보드 통합, 검색 데이터 모델 등을 설명 합니다.
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: f1085044f2147bec0910a87f8a6174c4648ef9b8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120153"
---
# <a name="working-with-tvos-text-and-search-fields-in-xamarin"></a>TvOS 텍스트 및 Xamarin에서 검색 필드를 사용 하 여 작업

Xamarin.tvOS 앱 (예: 사용자 Id 및 암호) 사용자에서 작은 텍스트를 요청할 수 있습니다, 필요한 경우 텍스트 필드를 사용 하 고 화상 키보드:

[![](text-fields-and-search-images/intro01.png "샘플 검색 필드")](text-fields-and-search-images/intro01.png#lightbox)

필요에 따라 앱의 콘텐츠 검색 필드를 사용 하 여 키워드 검색 기능을 제공할 수 있습니다.

[![](text-fields-and-search-images/intro02.png "샘플 검색 결과")](text-fields-and-search-images/intro02.png#lightbox)

이 문서에서는 Xamarin.tvOS 앱에서 텍스트 및 검색 필드를 사용 하 여 작업의 세부 정보를 설명 합니다.

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>텍스트 및 검색 필드에 대 한

필요한 경우, 위에서 설명한 대로 프로그램 Xamarin.tvOS 하 여 사용자에서 적은 양의 텍스트를 수집 하려면 하나 이상의 텍스트 필드를 제공할 수는 화면 (또는 사용자가 설치한 tvOS의 버전에 따라 선택적 bluetooth 키보드)입니다. 

또한 앱 (예:는 음악, 동영상 또는 사진 컬렉션) 사용자에 게 많은 양의 콘텐츠를 표시를 하는 경우 적은 양의 사용 가능한 항목 목록을 필터링 할 텍스트를 입력할 수 있는 검색 필드를 포함 하는 것이 좋습니다.

<a name="Text-Fields" />

## <a name="text-fields"></a>텍스트 필드

TvOS 텍스트 필드 표시 됩니다는 고정 높이, 둥근 모퉁이 항목 상자로 표시 되는 사용자가 클릭 하면 화상 키보드:

[![](text-fields-and-search-images/text01.png "텍스트 필드에서 tvOS")](text-fields-and-search-images/text01.png#lightbox)

가져가면 [포커스](~/ios/tvos/app-fundamentals/navigation-focus.md) 지정 된 텍스트 필드에 크게 증가 하 고 심층 그림자를 표시 합니다. 염두이 사용자 인터페이스를 디자인할 때 텍스트가 때 포커스 내 다른 UI 요소를 겹칠 수 해야 합니다.

Apple는 텍스트 필드를 사용 하 여 작업 하기 위한 다음 제안에 있습니다.

- **텍스트 항목 꼭 필요할 때만 사용 하 여** -특성으로 인해는 화상 키보드, 긴 텍스트 섹션을 입력 하거나 여러 텍스트 필드를 작성 하기란 지루할 뿐만 아니라 사용자가 있습니다. 선택 목록을 사용 하 여 텍스트 항목의 크기를 제한 하는 것이 좋습니다 또는 [단추가](~/ios/tvos/user-interface/buttons.md)합니다.
- **목적으로 통신 해 힌트를 사용 하 여** -텍스트 필드는 비어 있는 경우 자리 표시자 "힌트"를 표시할 수 있습니다. 해당 하는 대신 별도 레이블 텍스트 필드의 용도에 힌트를 사용 합니다.
- **적절 한 기본 키보드 형식 선택** -tvOS 텍스트 필드에 지정할 수 있는 여러 다른 용도의 키보드 형식을 제공 합니다. 예를 들어 전자 메일 주소 키보드 사용자가 최근에 입력 한 주소 목록에서 선택할 수 있도록 하 여 항목을 줄일 수 있습니다.
- **해당 하는 경우 보안 텍스트 필드를 사용 하 여** -보안 텍스트 필드는 실제 문자) (대신 점으로 입력 문자를 표시 합니다. 암호와 같은 중요 한 정보를 수집 하는 경우에 항상 Secure 텍스트 필드를 사용 합니다.

<a name="Keyboards" />

## <a name="keyboards"></a>키보드

클릭할 때마다 선형 사용자 인터페이스에서 텍스트 필드에서 화면의 바로 표시 됩니다. 사용자가 터치 화면을 사용 하 여 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote) 키보드에서 개별 문자를 선택 하 고 요청된 된 정보를 입력 하려면:

[![](text-fields-and-search-images/keyboard01.png "Siri 원격 키보드")](text-fields-and-search-images/keyboard01.png#lightbox)

현재 뷰를 텍스트 필드가 둘 이상일 경우는 **다음** 단추 다음 텍스트 필드에 사용자를 자동으로 표시 됩니다. A **수행** 단추 텍스트 항목 종료 되며 이전 화면으로 사용자를 반환 하는 마지막 텍스트 필드에 표시 됩니다. 

언제 든 지 사용자를 누를 수도 합니다 **메뉴** Siri 원격 텍스트 항목을 종료 되 고 이전 화면으로 다시 반환 하는 단추입니다.

Apple에 사용 하기 위한 다음 제안 사항을 화상 키보드

- **적절 한 기본 키보드 형식 선택** -tvOS 텍스트 필드에 지정할 수 있는 여러 다른 용도의 키보드 형식을 제공 합니다. 예를 들어 전자 메일 주소 키보드 사용자가 최근에 입력 한 주소 목록에서 선택할 수 있도록 하 여 항목을 줄일 수 있습니다.
- **키보드 액세서리 보기를 사용 하 여 해당 하는 경우** -표준 외에도 정보는 항상 표시 (선택 사항) 액세서리 뷰 (예: 이미지 또는 레이블)를 추가할 수는 화상 키보드 또는 텍스트 항목의 목적을 설명 사용자는 데 도움이 필요한 정보를 입력 합니다.

작업에 대 한 자세한 내용은 화상 키보드, Apple의를 참조 하세요 [UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType), [키보드 관리](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1), [데이터 입력에 대 한 사용자 지정 보기](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) 및[ Ios 프로그래밍 가이드 텍스트](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html) 설명서.

<a name="Search" />

## <a name="search"></a>검색

검색 필드 텍스트 필드를 제공 하는 특수 한 화면 있고 화상 키보드는 키보드 아래 표시 되는 항목의 컬렉션을 필터링 할 수 있습니다.

[![](text-fields-and-search-images/search01.png "샘플 검색 결과")](text-fields-and-search-images/search01.png#lightbox)

사용자가 문자 검색 필드에를 입력 하면 아래 검색의 결과 자동으로 반영 됩니다. 언제 든 지 사용자는 결과에 포커스를 이동 하 고 표시 되어 있는 항목 중 하나를 선택할 수 있습니다.

Apple 검색 필드를 사용 하 여 작업 하기 위한 다음 제안에 있습니다.

- **최근 검색 제공** -Siri 원격을 사용 하 여 텍스트를 입력 작업은 지루할 수 및 사용자 검색 요청을 반복 하 고, 키보드 영역에서 현재 결과 앞 최근 검색 결과의 섹션을 추가 하는 것이 좋습니다. 하려는 경향이 있습니다.
- **가능 하면 결과의 수를 제한할** -큰 항목 목록을 구문 분석 및 탐색, 반환 된 결과의 수를 제한 하는 것이 좋습니다. 사용자에 대 한 어려울 수 있습니다.
- **제공 검색 결과 필터링 하는 경우 적절 하 게** -자체 앱에서 제공 하는 콘텐츠에 적합 한 경우 반환 되는 검색 결과 추가로 필터링 할 수 있도록 범위 막대를 추가 하는 것이 좋습니다.

자세한 내용은 Apple의를 참조 하세요 [UISearchController 클래스 참조](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html)합니다.

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>텍스트 필드를 사용 하 여 작업

Xamarin.tvOS 앱에서 텍스트 필드를 사용 하는 가장 쉬운 방법은 iOS 디자이너를 사용 하 여 사용자 인터페이스 디자인에 추가할 경우

다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. 에 **Solution Pad**를 두 번 클릭 합니다 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 하나 이상의 끌기 **텍스트 필드** int 보기로 디자인 화면: 

    [![](text-fields-and-search-images/text02.png "텍스트 필드")](text-fields-and-search-images/text02.png#lightbox)
1. 선택는 **텍스트 필드** 고유한 제공 하는 각 **이름** 에 **위젯** 탭을 **Properties Pad**: 

    [![](text-fields-and-search-images/text03.png "Properties Pad의 위젯 탭")](text-fields-and-search-images/text03.png#lightbox)
1. 에 **텍스트 필드** 섹션인 같은 요소를 정의할 수 있습니다 합니다 **자리 표시자** 힌트 및 기본 **값**: 

    [![](text-fields-and-search-images/text04.png "텍스트 필드 섹션")](text-fields-and-search-images/text04.png#lightbox)
1. 와 같은 속성을 정의 하려면 아래로 스크롤하여 **맞춤법 검사**를 **첫 글자를 대문자로** 및 기본값 **키보드 형식**: 

    [![](text-fields-and-search-images/text05.png "맞춤법 검사, 대/소문자 및 기본 키보드 유형")](text-fields-and-search-images/text05.png#lightbox) 
1. 스토리 보드의 변경 내용을 저장 합니다.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. 편집하기 위해 **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭하여 엽니다.
1. 하나 이상의 끌기 **텍스트 필드** int 보기로 디자인 화면: 

    [![](text-fields-and-search-images/text02-vs.png "텍스트 필드")](text-fields-and-search-images/text02-vs.png#lightbox)
1. 선택는 **텍스트 필드** 고유한 제공 하는 각 **이름** 에 **위젯** 탭을 **속성 탐색기**: 

    [![](text-fields-and-search-images/text03-vs.png "위젯 탭")](text-fields-and-search-images/text03-vs.png#lightbox)
1. 에 **텍스트 필드** 섹션인 같은 요소를 정의할 수 있습니다 합니다 **자리 표시자** 힌트 및 기본 **값**: 

    [![](text-fields-and-search-images/text04-vs.png "텍스트 필드 섹션")](text-fields-and-search-images/text04-vs.png#lightbox)
1. 와 같은 속성을 정의 하려면 아래로 스크롤하여 **맞춤법 검사**를 **첫 글자를 대문자로** 및 기본값 **키보드 형식**: 

    [![](text-fields-and-search-images/text05-vs.png "맞춤법 검사, 대/소문자 및 기본 키보드 유형")](text-fields-and-search-images/text05-vs.png#lightbox) 
1. 스토리 보드의 변경 내용을 저장 합니다.
    
-----

코드에서 가져오기 또는 사용 하 여 텍스트 필드의 값을 설정할 수는 `Text` 속성:

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

필요에 따라 사용할 수 있습니다 합니다 `Started` 및 `Ended` 텍스트 항목의 시작과 끝에 응답할 이벤트 텍스트 필드입니다.

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>검색 필드를 사용 하 여 작업

Xamarin.tvOS 앱에서 검색 필드를 사용 하는 가장 쉬운 방법은 인터페이스 디자이너를 사용 하 여 사용자 인터페이스 디자인에 추가할 경우

다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
1. 에 **Solution Pad**를 두 번 클릭 합니다 `Main.storyboard` 파일을 편집용으로 엽니다.
1. 사용자의 검색 결과 제공 하는 스토리 보드에 새 컬렉션 뷰 컨트롤러를 끌어 옵니다. 

    [![](text-fields-and-search-images/search02.png "컬렉션 뷰 컨트롤러")](text-fields-and-search-images/search02.png#lightbox)
1. 에 **위젯** 탭을 **Properties Pad**를 사용 하 여 `SearchResultsViewController` 에 대 한를 **클래스** 및 `SearchResults` 에 대 한를 **스토리 보드 ID**: 

    [![](text-fields-and-search-images/search03.png "위젯 탭")](text-fields-and-search-images/search03.png#lightbox)
1. 선택 된 **셀 프로토타입** 디자인 화면에서 합니다.
1. 에 **위젯** 탭을 **속성 탐색기**를 사용 하 여 `SearchResultCell` 에 대 한를 **클래스** 및 `ImageCell` 에 대 한는 **식별자**: 

    [![](text-fields-and-search-images/search04.png "위젯 탭")](text-fields-and-search-images/search04.png#lightbox)
1. 레이아웃 디자인의는 **셀 프로토타입** 고유를 사용 하 여 각 요소를 노출 **이름** 에 **위젯** 탭은 **속성 탐색기**: 

    [![](text-fields-and-search-images/search05.png "셀 프로토타입 디자인 레이아웃")](text-fields-and-search-images/search05.png#lightbox)
1. 스토리 보드의 변경 내용을 저장 합니다.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. 편집하기 위해 **솔루션 탐색기**에서 `Main.storyboard` 파일을 두 번 클릭하여 엽니다.
1. 사용자의 검색 결과 제공 하는 스토리 보드에 새 컬렉션 뷰 컨트롤러를 끌어 옵니다. 

    [![](text-fields-and-search-images/seach02-vs.png "컬렉션 뷰 컨트롤러")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. 에 **위젯** 탭을 **속성 탐색기**를 사용 하 여 `SearchResultsViewController` 에 대 한를 **클래스** 및 `SearchResults` 에 대 한는 **스토리 보드 ID**: 

    [![](text-fields-and-search-images/search03-vs.png "위젯 탭")](text-fields-and-search-images/search03-vs.png#lightbox)
1. 선택 된 **셀 프로토타입** 디자인 화면에서 합니다.
1. 에 **위젯** 탭을 **속성 탐색기**를 사용 하 여 `SearchResultCell` 에 대 한를 **클래스** 및 `ImageCell` 에 대 한는 **식별자**: 

    [![](text-fields-and-search-images/search04-vs.png "위젯 탭")](text-fields-and-search-images/search04-vs.png#lightbox)
1. 레이아웃 디자인의는 **셀 프로토타입** 고유를 사용 하 여 각 요소를 노출 **이름** 에 **위젯** 탭은 **속성 탐색기**: 

    [![](text-fields-and-search-images/search05-vs.png "셀 프로토타입 디자인 레이아웃")](text-fields-and-search-images/search05-vs.png#lightbox)
1. 스토리 보드의 변경 내용을 저장 합니다.
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>데이터 모델을 제공 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

그런 다음 역할을 사용자에 대 한 검색 결과 대 한 데이터 모델 클래스를 제공 해야 합니다. 에 **솔루션 탐색기**, 프로젝트 이름을 마우스 오른쪽 단추로 **추가** > **새 파일...**   >  **일반** > **빈 클래스** 하 고 제공 된 **이름**: 

[![](text-fields-and-search-images/search06.png "빈 클래스를 선택 하 고 이름을 제공합니다")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

그런 다음 역할을 사용자에 대 한 검색 결과 대 한 데이터 모델 클래스를 제공 해야 합니다. 에 **솔루션 탐색기**, 프로젝트 이름을 마우스 오른쪽 단추로 **추가** > **새 항목...**   >  **Apple** > **기타** > **클래스** 제공 하 고는 **이름**: 

[![](text-fields-and-search-images/search06-vs.png "클래스를 선택 하 고 이름을 제공합니다")](text-fields-and-search-images/search06-vs.png#lightbox)

-----

예를 들어 그림 제목 및 키워드의 컬렉션을 검색할 수 있도록 하는 앱을 다음과 같습니다.

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

현재 위치에서 데이터 모델을 사용 하 여 편집 합니다 **프로토타입 셀** (`SearchResultViewCell.cs`) 있도록 및 확인은 다음:

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

`UpdateUI` 의 개별 필드를 표시 하려면 사용할 메서드를 **PictureInformation** 항목 (합니다 `PictureInfo` 속성) 명명 된 UI 요소에 있는 각 시간 속성이 업데이트 됩니다. 이미지와 그림을 사용 하 여 연결 된 제목 예를 들어 있습니다.

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>컬렉션 뷰 컨트롤러

그런 다음 검색 결과 컬렉션 뷰 컨트롤러를 편집 (`SearchResultsViewController.cs`), 다음과 같이 표시 되도록 및:

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

첫 번째는 `IUISearchResultsUpdating` 인터페이스는 사용자가 업데이트 되 고 검색 컨트롤러 필터를 처리 하는 클래스에 추가 됩니다.

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

ID를 지정할 수도 정의 된 상수는 **프로토타입 셀** (일치 하는 위의 인터페이스 디자이너에서 정의 된 ID)는 나중에 때 사용할 새 셀을 요청 하는 컬렉션 컨트롤러:

```csharp
public const string CellID = "ImageCell";
```

전체 목록을 검색할 검색어 필터 항목에 대 한 저장소 만들어집니다 및 해당 용어를 일치 하는 항목 목록:

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

경우는 `SearchFilter` 는 일치 하는 항목 목록이 업데이트 되어 변경 및 컬렉션 뷰에서 콘텐츠를 다시 로드 합니다. `FindPictures` 새 검색어와 일치 하는 항목을 찾기 위한 루틴 담당 합니다.

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

값을 `SearchFilter` (결과 컬렉션 뷰는 업데이트를) 업데이트할 수는 사용자 검색 컨트롤러에서 필터를 변경 하는 경우:

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

`PopulatePictures` 메서드는 처음에 사용 가능한 항목의 컬렉션을 채웁니다.

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

이 예제의 경우 샘플 데이터의 모든 만들어집니다 메모리에서 컬렉션 뷰 컨트롤러를 로드 하는 경우. 실제 앱에서는 데이터베이스나 웹 서비스에서이 데이터를 읽을 것은 및에서 유지 하는 데 필요한 때에 사용 하 고 Apple TV를 오버런 제한 된 메모리입니다.

합니다 `NumberOfSections` 및 `GetItemsCount` 메서드는 일치 항목 수를 제공 합니다.

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

합니다 `GetCell` 메서드는 새 반환 **프로토타입 셀** (기반는 `CellID` 스토리 보드에서 위에 정의 된) 컬렉션 뷰에서의 각 항목에 대 한:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

`WillDisplayCell` 구성할 수 있도록 표시 되 고 셀 전에 메서드를 호출 합니다.

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

`DidUpdateFocus` 메서드 결과 컬렉션 뷰의 항목 강조 표시 하는 대로 사용자에 게 시각적 피드백을 제공 합니다.

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

마지막으로 `ItemSelected` 메서드 결과 컬렉션 뷰의 (Siri 원격을 사용 하 여 터치 화면에서 클릭) 항목을 선택 하면 사용자를 처리 합니다.

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

검색 필드를으로 표시 되 면 모달 대화 상자 보기 (위에 호출 뷰)를 사용 하 여는 `DismissViewController` 항목을 선택 하면 검색 뷰를 해제 하는 방법입니다. 예를 들어 검색 필드는 해당 되지 해제 중인 여기 하므로 탭 보기 탭의 내용으로 표시 됩니다.

컬렉션 뷰에 대 한 자세한 내용은 참조 하십시오 우리의 [컬렉션 뷰를 사용 하 여 작업](~/ios/tvos/user-interface/collection-views.md) 설명서.

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>검색 필드를 표시합니다.

두 가지 주요는 검색 필드 (및 해당 관련 화상 키보드와 검색 결과) tvOS에서 사용자에 게 표시 될 수 있습니다. 

- **모달 대화 상자 뷰가** -보기 및 전체 화면 모달 대화 뷰로 뷰 컨트롤러는 검색 필드 현재 위에 표시 될 수 있습니다. 단추 또는 다른 UI 요소를 클릭 하는 사용자에 대 한 응답에서 일반적으로 수행 됩니다. 사용자가 검색 결과에서 항목 선택 대화 상자 해제 되었습니다.
- **내용 보기** -The 검색 필드는 지정 된 뷰를 직접 포함 합니다. 예를 들어의 콘텐츠로 탭 뷰 컨트롤러의 검색 탭 합니다.

그림 위에 지정 된 검색 가능한 목록 예에 대 한 검색 필드에서 검색 탭 내용 보기도 표시 됩니다 및 검색 탭 뷰 컨트롤러는 다음과 같습니다.

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

일치 하는 상수는 먼저 정의 합니다 **스토리 보드 식별자** 인터페이스 디자이너에서 검색 결과 컬렉션 뷰 컨트롤러에 할당 된:

```csharp
public const string SearchResultsID = "SearchResults";
```

다음으로 `ShowSearchController` 메서드 새 검색 뷰 컬렉션 컨트롤러를 만들고 표시 합니다.이 기능이 필요 했습니다.

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

위의 메서드를 한 번에 `SearchResultsViewController` 스토리 보드에서 인스턴스화된 새 `UISearchController` 검색 필드를 표시 하 고 사용자에 게 화상 키보드에 만들어집니다. 검색 결과 컬렉션 (정의 된 대로 `SearchResultsViewController`)이이 바로 아래에 표시 됩니다.

다음으로 `SearchBar` 같은 정보를 사용 하 여 구성 됩니다 합니다 **자리 표시자** 힌트입니다. 그러나 되는 검색 유형에 대 한 사용자에 게 정보를 제공 합니다.

그런 다음 검색 필드는 두 가지 방법 중 하나에서 사용자에 게 표시 됩니다.

- **모달 대화 상자 뷰가** - `PresentViewController` 메서드 기존 보기를 통해 검색을 제공 하는 전체 화면.
- **내용 보기** - `UISearchContainerViewController` 검색 컨트롤러를 포함 하도록 만들어집니다. A `UINavigationController` 탐색 컨트롤러는 뷰 컨트롤러에 추가 검색 컨테이너를 포함 하기 위해 만들어진 `AddChildViewController (navController)`, 및 표시 되는 보기 `View.Add (navController.View)`합니다.

마지막으로, 다시 기반 프레젠테이션 형식 중 하나는 `ViewDidLoad` 또는 `ViewDidAppear` 메서드를 호출 합니다는 `ShowSearchController` 검색 사용자에 게 제공 하는 방법.

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

때 앱이 실행 되 고 사용자가 선택한 검색 탭을 사용자에 게 전체 필터링 되지 않은 항목 목록이 표시 됩니다.

[![](text-fields-and-search-images/intro02.png "기본 검색 결과")](text-fields-and-search-images/intro02.png#lightbox)

사용자는 검색 용어를 입력 하기 시작, 결과 목록 용어도 필터링 되며 자동으로 업데이트:

[![](text-fields-and-search-images/intro03.png "필터링 된 검색 결과")](text-fields-and-search-images/intro03.png#lightbox)

언제 든 지 사용자 검색 결과에서 항목에 포커스를 전환 하 고 선택 Siri 원격 터치 화면 클릭 수 있습니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 디자인 및 Xamarin.tvOS 앱 내에서 텍스트 검색 필드와도 협력 검사가 수행 합니다. 인터페이스 디자이너에서 텍스트 및 검색 컬렉션 콘텐츠를 만드는 방법에 알아보았습니다 및 검색 필드 tvOS에서 사용자에 게 표시 될 수 있는 두 가지 방법에 알아보았습니다.



## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
