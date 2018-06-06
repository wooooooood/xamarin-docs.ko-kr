---
title: Xamarin.iOS의 지도
description: 이 문서에는 iOS MapKit 프레임 워크 및 Xamarin.iOS 함께 사용 하는 방법을 설명 합니다. 지도, 이동 및 확대/축소 고 작업을 사용자 위치, 주석 추가, 설명선 및 오버레이, 작업을 로컬 검색을 수행 하는 스타일을 추가 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3649c8eb9c8c1a82940b8e2eece7d2bfd005d024
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790232"
---
# <a name="maps-in-xamarinios"></a>Xamarin.iOS의 지도

지도 모바일 운영 체제의 일반적인 기능입니다. iOS는 기본적으로 지도 키트 프레임 워크를 통해 매핑을 지원 합니다. 지도 키트 응용 프로그램 다양 하 고 대화형 지도 쉽게 추가할 수 있습니다. 다양 한 방법으로 지도에 위치를 표시 하는 주석을 추가 하 고 그래픽의 임의 셰이프를 겹치게 등 이러한 맵은 사용자 지정할 수 있습니다. 도 맵 키트에는 장치의 현재 위치를 표시 하는 데 기본적으로 지원 합니다.

## <a name="adding-a-map"></a>지도 추가

추가 하 여 구현 응용 프로그램에 지도 추가 `MKMapView` 아래와 같이 보기 계층에 인스턴스:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` 이 `UIView` 지도 표시 하는 하위 클래스입니다. 위의 코드를 사용 하 여 지도 추가 하기만 하면 대화형 지도 생성 합니다.

 ![](images/00-map.png "샘플 맵")

## <a name="map-style"></a>지도 유형

 `MKMapView` 맵의 3 다양 한 스타일을 지원합니다. 지도 스타일을 적용 하려면 설정 하기만 `MapType` 속성의 값을는 `MKMapType` 열거형:
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  다음 스크린 샷에서 사용할 수 있는 다른 맵 스타일을 보여 줍니다.

 ![](images/01-mapstyles.png "이 스크린 샷 사용할 수 있는 다른 맵 스타일 표시")

## <a name="panning-and-zooming"></a>이동 및 확대/축소

 `MKMapView` 와 같은 맵 대화형 기능에 대 한 지원이 포함 됩니다.

-  축소 제스처를 통해 확대/축소
-  팬 제스처를 통해 이동합니다.


이러한 기능을 설정 하거나 단순히 설정 하 여 해제할 수 있습니다는 `ZoomEnabled` 및 `ScrollEnabled` 의 속성은 `MKMapView` 인스턴스, 기본 값은 둘 다에 대해 true입니다. 예를 들어 정적 지도 표시 하려면 단순히 적절 한 속성을 false로 설정 합니다.

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>사용자 위치

사용자 상호 작용 외에도 `MKMapView` 장치의 위치를 표시 하기 위한 기본 제공 지원이 합니다. 이렇게 하기를 사용 하는 *코어 위치* 프레임 워크입니다. 사용자의 위치에 액세스 하기 전에 사용자를 표시 해야 합니다. 이 위해의 인스턴스를 만들 `CLLocationManager` 호출 `RequestWhenInUseAuthorization`합니다.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

IOS 8.0, 호출을 시도 하기 전에 버전에서 유의 `RequestWhenInUseAuthorization` 에서 오류가 발생 합니다. IOS의 버전과 8 이전 버전을 지원 하려는 경우 해당 호출을 만들기 전에 확인 해야 합니다.

사용자의 위치에 액세스를 수정 해야 **Info.plist**합니다. 위치 데이터와 관련된 다음 키를 설정해야 합니다.

- **NSLocationWhenInUseUsageDescription** - 사용자가 앱과 상호 작용하는 동안 사용자의 위치에 액세스하는 경우
- **NSLocationAlwaysUsageDescription** - 앱이 백그라운드에서 사용자의 위치에 액세스하는 경우

열어 해당 키를 추가할 수 있습니다 **Info.plist** 선택 하 고 *소스* 편집기의 맨 아래에 있습니다.

업데이트 한 후 **Info.plist** 사용자에 게 해당 위치에 액세스할 수 있는 권한 묻는 메시지가 표시 하 고, 사용자의 위치를 설정 하 여 지도에 표시할 수 있습니다는 `ShowsUserLocation` 속성을 true로:

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "허용 위치 액세스 경고")
 
## <a name="annotations"></a>주석

 `MKMapView` 지도에 주석으로 알려진 이미지 표시도 지원 합니다. 사용자 지정 이미지 또는 시스템에서 정의한 핀의 다양 한 색 수 있습니다. 예를 들어 다음 스크린샷은 pin으로 지도 및 이미지를 사용자 지정:

 ![](images/03-annotations.png "이 스크린샷은 pin으로 지도 및 이미지를 사용자 지정")

### <a name="adding-an-annotation"></a>주석 추가

주석 자체는 두 개의 구성 됩니다.

-  `MKAnnotation` 제목, 주석의 위치 등 주석에 대 한 모델 데이터를 포함 하는 개체입니다.
-  `MKAnnotationView` 를 표시 하 고 필요에 따라 사용자가 주석을 누를 때 표시 되는 설명선 이미지를 포함 하 합니다.


맵 키트에서 iOS 위임 패턴을 사용 하 여 주석을 추가 하는 지도 여기서는 `Delegate` 속성의는 `MKMapView` 의 인스턴스로 설정 되는 `MKMapViewDelegate`합니다. 반환 하는 일을 담당 하는이 구현에이 대리자는 `MKAnnotationView` 주석에 대 한 합니다.

주석을 추가 하려면 먼저 주석이 추가 호출 하 여 `AddAnnotations` 에 `MKMapView` 인스턴스:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

주석의 위치는 지도에 표시 되는 경우는 `MKMapView` 해당 대리자를 호출 합니다 `GetViewForAnnotation` 가져올 메서드를는 `MKAnnotationView` 표시 합니다.

다음 코드에서 제공 하 고 시스템을 반환 하는 예를 들어 `MKPinAnnotationView`:

```csharp
string pId = "PinAnnotation";

public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
{
    if (annotation is MKUserLocation)
        return null;

    // create pin annotation view
    MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);

    if (pinView == null)
        pinView = new MKPinAnnotationView (annotation, pId);

    ((MKPinAnnotationView)pinView).PinColor = MKPinAnnotationColor.Red;
    pinView.CanShowCallout = true;

    return pinView;
}
```

### <a name="reusing-annotations"></a>주석 다시 사용

메모리를 절약 하기 위해 `MKMapView` 주석 보기의 테이블 셀이 다시 사용 하는 방식과 유사 하 게 다시 사용 하기 위해 풀링할 수 있습니다. 에 대 한 호출을 완료 풀에서 주석 보기 가져오기 `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>설명선 표시

앞서 언급 했 듯이 주석 설명선을 선택적으로 표시할 수 있습니다. 설명선 단순히 표시 하려면 설정 `CanShowCallout` 에 true로는 `MKAnnotationView`합니다. 이 인해 표시 된 대로 주석이 탭이 수행 되는 경우 표시 되는 주석의 제목:

 ![](images/04-callout.png "표시 되 고 주석 제목")

### <a name="customizing-the-callout"></a>설명선 사용자 지정

설명선 아래와 같이 왼쪽 및 오른쪽 액세서리 보기에 표시할에 정의할 수 또한:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

이 코드는 다음 설명선 발생합니다.

 ![](images/05-callout-accessories.png "예제 설명선")

오른쪽 액세서리를 눌러 사용자를 처리 하려면 단순히 구현는 `CalloutAccessoryControlTapped` 에서 메서드는 `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>오버레이

지도에 계층 그래픽에 또 다른 방법은 오버레이 사용 중입니다. 오버레이 함에 따라 확장 지도와 확대 하는 그리기 그래픽 콘텐츠를 지원 합니다. iOS에 오버레이 비롯 한 여러 유형에 대 한 지원을 제공 합니다.

-  다각형-지도에 몇 가지 영역을 강조 표시 하는 데 주로 사용 됩니다.
-  폴리라인을-때 경로 보는 경우가 많습니다.
-  원-맵의 순환 영역을 강조 표시 하는 데 사용 합니다.


또한 사용자 지정 오버레이 세부적으로, 사용자 지정 그리기 코드를 임의의 기 하 도형을 표시 하도록 만들 수 있습니다. 예를 들어 날씨 방사형 사용자 지정 오버레이 될 것입니다.

#### <a name="adding-an-overlay"></a>오버레이 추가

주석와 마찬가지로, 오버레이 추가 2 부를 수행 합니다.

-  오버레이 대 한 모델 개체를 만들고 추가 하는 `MKMapView` 합니다.
-  에 오버레이 대 한 보기 만들기는 `MKMapViewDelegate` 합니다.


오버레이 대 한 모델 하나일 수 있습니다 `MKShape` 하위 클래스입니다. Xamarin.iOS 포함 `MKShape` 다각형, 폴리라인을 및 원, 서브 클래스를 통해는 `MKPolygon`, `MKPolyline` 및 `MKCircle` 각각.

추가 하려면 다음 코드를 사용 하는 예를 들어 한 `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

오버레이 대 한 뷰는는 `MKOverlayView` 에서 반환 되는 인스턴스는 `GetViewForOverlay` 에 `MKMapViewDelegate`합니다. 각 `MKShape` 는 해당 `MKOverlayView` 는 지정 된 도형 표시 하는 방법을 알고 있는 합니다. 에 대 한 `MKPolygon` 는 `MKPolygonView`합니다. 마찬가지로, `MKPolyline` 에 해당 `MKPolylineView`, 및에 대 한 `MKCircle` 는 `MKCircleView`합니다.

예를 들어 다음 코드 반환은 `MKCircleView` 에 대 한 프로그램 `MKCircle`:

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

이 같이 지도에 원을 표시 합니다.

 ![](images/06-circle-overlay.png "지도에 표시 하는 원")

## <a name="local-search"></a>로컬 검색

iOS 로컬 검색 비동기 지정된 지리적 지역에 관심 지점 검색할 수 있는 지도 키트를 사용 하 여 API를 포함 합니다.

응용 프로그램 로컬 검색을 수행 하려면 다음이 단계를 수행 해야 합니다.

1.  만들 `MKLocalSearchRequest` 개체입니다.
1.  만들기는 `MKLocalSearch` 에서 개체는 `MKLocalSearchRequest` 합니다.
1.  호출의 `Start` 에서 메서드는 `MKLocalSearch` 개체입니다.
1.  검색 된 `MKLocalSearchResponse` 콜백에서 개체입니다.


로컬 검색 API 자체는 없는 사용자 인터페이스를 제공합니다. 사용할 맵도 필요 하지 않습니다. 그러나 로컬 검색으로 사용 하려면 응용 프로그램 해야 검색 쿼리를 지정 하 고 결과 표시할 수 있는 방법을 제공 합니다. 또한 결과 위치 데이터를 포함할 수 있으므로 종종 좋을 지도에 표시 하려면.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>로컬 검색 UI 추가

검색 입력을 허용 하는 한 가지 방법은 인 한 `UISearchBar`에서 제공 하는 `UISearchController` 테이블의 결과 표시 됩니다.

다음 코드에서는 추가 `UISearchController` (있으며 검색 표시줄 속성)에 `ViewDidLoad` 방식의 `MapViewController`:

```csharp
//Creates an instance of a custom View Controller that holds the results
var searchResultsController = new SearchResultsViewController (map);

//Creates a search controller updater
var searchUpdater = new SearchResultsUpdator ();
searchUpdater.UpdateSearchResults += searchResultsController.Search;
            
//add the search controller
searchController = new UISearchController (searchResultsController) {
                SearchResultsUpdater = searchUpdater
            };

//format the search bar
searchController.SearchBar.SizeToFit ();
searchController.SearchBar.SearchBarStyle = UISearchBarStyle.Minimal;
searchController.SearchBar.Placeholder = "Enter a search query";

//the search bar is contained in the navigation bar, so it should be visible
searchController.HidesNavigationBarDuringPresentation = false;

//Ensure the searchResultsController is presented in the current View Controller 
DefinesPresentationContext = true;

//Set the search bar in the navigation bar
NavigationItem.TitleView = searchController.SearchBar;

```csharp
Note that you are responsible for incorporating the search bar object into the user interface. In this example, we assigned it to the TitleView of the navigation bar, but if you do not use a navigation controller in your application you will have to find another place to display it.

In this code snippet, we created another custom view controller – `searchResultsController` –  that displays the search results and then we used this object to create our search controller object. We also created a new search updater, which becomes active when the user interacts with the search bar. It receives notifications about searches with each keystroke and is responsible for updating the UI.
We will take a look at how to implement both the `searchResultsController` and the `searchResultsUpdater` later in this guide.

This results in a search bar displayed over the map as shown below:

 ![](images/07-searchbar.png "A search bar displayed over the map")
 


### Displaying the Search Results

To display search results, we need to create a custom View Controller; normally a `UITableViewController`. As shown above, the `searchResultsController` is passed to the constructor of the `searchController` when it is being created.
The following code is an example of how to create this custom View Controller:

```csharp
public class SearchResultsViewController : UITableViewController
{
    static readonly string mapItemCellId = "mapItemCellId";
    MKMapView map;

    public List<MKMapItem> MapItems { get; set; }

    public SearchResultsViewController (MKMapView map)
    {
        this.map = map;

        MapItems = new List<MKMapItem> ();
    }

    public override nint RowsInSection (UITableView tableView, nint section)
    {
        return MapItems.Count;
    }

    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        var cell = tableView.DequeueReusableCell (mapItemCellId);

        if (cell == null)
            cell = new UITableViewCell ();

        cell.TextLabel.Text = MapItems [indexPath.Row].Name;
        return cell;
    }

    public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
    {
        // add item to map
        CLLocationCoordinate2D coord = MapItems [indexPath.Row].Placemark.Location.Coordinate;
        map.AddAnnotations (new MKPointAnnotation () {
            Title = MapItems [indexPath.Row].Name,
            Coordinate = coord
        });

        map.SetCenterCoordinate (coord, true);

        DismissViewController (false, null);
    }

    public void Search (string forSearchString)
    {
        // create search request
        var searchRequest = new MKLocalSearchRequest ();
        searchRequest.NaturalLanguageQuery = forSearchString;
        searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

        // perform search
        var localSearch = new MKLocalSearch (searchRequest);

        localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
            if (response != null && error == null) {
                this.MapItems = response.MapItems.ToList ();
                this.TableView.ReloadData ();
            } else {
                Console.WriteLine ("local search error: {0}", error);
            }
        });


    }
}
```

### <a name="updating-the-search-results"></a>검색 결과 업데이트

`SearchResultsUpdater` 간의 중개자 역할는 `searchController`의 검색 창 및 검색 결과. 

이 예제에서 검색 메서드를 먼저 만든 해야는 `SearchResultsViewController`합니다. 만들어야이 작업을 수행 하는 `MKLocalSearch` 개체를 사용 하 여에 대 한 검색을 실행할는 `MKLocalSearchRequest`에 전달 된 콜백에서 결과가 검색 되는 `Start` 의 메서드는 `MKLocalSearch` 개체입니다. 다음 결과가 반환 되는 `MKLocalSearchResponse` 의 배열을 포함 하는 개체 `MKMapItem` 개체:

```csharp
public void Search (string forSearchString)
{
    // create search request
    var searchRequest = new MKLocalSearchRequest ();
    searchRequest.NaturalLanguageQuery = forSearchString;
    searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

    // perform search
    var localSearch = new MKLocalSearch (searchRequest);

    localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
        if (response != null && error == null) {
            this.MapItems = response.MapItems.ToList ();
            this.TableView.ReloadData ();
        } else {
            Console.WriteLine ("local search error: {0}", error);
        }
    });


}
```

그런 다음 우리의 `MapViewController` 의 사용자 지정 구현을 만들겠습니다 `UISearchResultsUpdating`에 할당 되는 `SearchResultsUpdater` 속성의 우리의 `searchController` 에 [로컬 검색 UI 추가](#Adding_a_Local_Search_UI) 섹션:

```csharp
public class SearchResultsUpdator : UISearchResultsUpdating
{
    public event Action<string> UpdateSearchResults = delegate {};

    public override void UpdateSearchResultsForSearchController (UISearchController searchController)
    {
        this.UpdateSearchResults (searchController.SearchBar.Text);
    }
}
```

웹 서비스 구현에 주석을 추가 지도 결과에서 항목을 선택 하는 경우 아래와 같이:

 ![](images/08-search-results.png "결과에서 항목을 선택 하는 경우 지도에 추가 하는 주석")
 
> [!IMPORTANT]
> `UISearchController` iOS 8에서에서 구현 되었습니다. 이 이전의 장치를 지원 하려는 경우 사용 해야 합니다 `UISearchDisplayController`합니다.



## <a name="summary"></a>요약

이 문서를 검사 하는 *지도* *키트* ios 프레임 워크입니다. 방법을 살펴 것 먼저는 `MKMapView` 클래스를 사용 하면 대화형 지도 응용 프로그램에 포함 되도록 합니다. 그런 다음 추가로 주석 및 오버레이 사용 하 여 맵을 사용자 지정 하는 방법을 보여 줍니다. 마지막으로, 6.1, ios 지도 키트에 추가 된 로컬 검색 기능을 검사 사용 방법을 보여 주는 관심 지점에 대 한 위치를 기반으로 쿼리를 수행 하 고 지도에 추가 합니다.

## <a name="related-links"></a>관련 링크

- [SearchController](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
- [MapDemo (샘플)](https://developer.xamarin.com/samples/monotouch/MapDemo)
