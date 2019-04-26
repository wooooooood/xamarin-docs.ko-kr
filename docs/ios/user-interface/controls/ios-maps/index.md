---
title: Xamarin.iOS의 지도
description: 이 문서에서는 iOS MapKit 프레임 워크와 Xamarin.iOS와 함께 사용 되는 방법을 설명 합니다. 지도, 이동 및 확대/축소, 사용자 위치를 사용 하 여 작업, 주석 추가, 설명선 및 오버레이 사용 하 여 작업 및 로컬 검색을 수행 하는 스타일을 추가 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: c61b5a8bd99afda5e8fbeea44e3362574fa7feea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61226730"
---
# <a name="maps-in-xamarinios"></a>Xamarin.iOS의 지도

Maps는 모든 최신 모바일 운영 체제의 일반적인 기능. iOS에는 Map 키트 프레임 워크를 통해 고유 하 게 매핑 지원을 제공합니다. Map 키트를 사용 하 여 응용 프로그램 풍부 하 고 대화형 지도 쉽게 추가할 수 있습니다. 이러한 맵은 다양 한 위치를 지도에 표시할 주석을 추가 임의 셰이프의 그래픽을 오버레이 등의 방법으로 사용자 지정할 수 있습니다. Map 키트도에 장치의 현재 위치를 표시 하기 위한 기본 제공 지원

## <a name="adding-a-map"></a>지도 추가합니다.

추가 하 여 수행 됩니다 응용 프로그램에 지도 추가 `MKMapView` 아래와 같이 뷰 계층 구조에 인스턴스:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` 가 `UIView` 지도 표시 하는 하위 클래스입니다. 위의 코드를 사용 하 여 지도 추가 하기만 하면 대화형 지도 생성 합니다.

 ![](images/00-map.png "샘플 맵")

## <a name="map-style"></a>지도 스타일

 `MKMapView` 맵의 3 다양 한 스타일을 지원합니다. 지도 스타일을 적용 하려면 설정 하기만 합니다 `MapType` 속성의 값을는 `MKMapType` 열거형:
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  다음 스크린샷에서 사용할 수 있는 다른 맵 스타일을 보여 줍니다.

 ![](images/01-mapstyles.png "이 스크린샷에서 사용할 수 있는 다른 맵 스타일 표시")

## <a name="panning-and-zooming"></a>팬 및 확대/축소

 `MKMapView` 같은 맵 대화형 기능에 대 한 지원을 포함 되어 있습니다.

-  축소 제스처를 통해 확대/축소
-  팬 제스처를 통해 이동합니다.


이 기능이 사용 하도록 설정 하거나 설정 하 여 해제할 수는 `ZoomEnabled` 및 `ScrollEnabled` 의 속성을 `MKMapView` 인스턴스, 기본 값이 모두 true입니다. 예를 들어, 정적 지도 표시 하려면 단순히 적절 한 속성을 false로 설정 합니다.

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>사용자 위치

사용자 상호 작용 하는 것 외에도 `MKMapView` 기능도 장치의 위치를 표시 하기 위한 기본 제공 지원. 이 작업을 사용 합니다 *Core 위치* 프레임 워크입니다. 사용자의 위치에 액세스 하기 전에 사용자를 표시 해야 합니다. 이렇게 하려면의 인스턴스를 만듭니다 `CLLocationManager` 호출 `RequestWhenInUseAuthorization`합니다.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

버전 8.0 호출 하려고 하기 전에 iOS에서 `RequestWhenInUseAuthorization` 오류가 발생 합니다. IOS의 버전과 8 이전 버전을 지원 하려는 경우 해당 호출을 수행 하기 전에 확인 해야 합니다.

수정 해야 사용자의 위치에 액세스 **Info.plist**합니다. 위치 데이터와 관련된 다음 키를 설정해야 합니다.

- **NSLocationWhenInUseUsageDescription** - 사용자가 앱과 상호 작용하는 동안 사용자의 위치에 액세스하는 경우
- **NSLocationAlwaysUsageDescription** - 앱이 백그라운드에서 사용자의 위치에 액세스하는 경우

열어 해당 키를 추가할 수 있습니다 **Info.plist** 를 선택 하 고 *원본* 편집기의 맨 아래에 있습니다.

업데이트 한 후 **Info.plist** 해당 위치에 액세스할 수 있는 권한 묻는 메시지가 표시 되 고 설정 하 여 사용자의 위치를 지도에 표시할 수 있습니다는 `ShowsUserLocation` 속성을 true로 합니다.

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "허용 위치 액세스 경고")
 
## <a name="annotations"></a>주석

 `MKMapView` 지도에 주석으로 알려진 표시 이미지도 지원 합니다. 사용자 지정 이미지 또는 다양 한 색의 시스템 정의 핀 수 있습니다. 예를 들어 다음 스크린샷에서 pin 사용 하 여 맵 및 사용자 지정 이미지:

 ![](images/03-annotations.png "이 스크린샷에서 pin 사용 하 여 맵 및 사용자 지정 이미지")

### <a name="adding-an-annotation"></a>주석 추가

자체 주석의 두 부분이 있습니다.

-  `MKAnnotation` 제목과 주석의 위치 된 주석에 대 한 모델 데이터를 포함 하는 개체입니다.
-  `MKAnnotationView` 를 표시 하 고 필요에 따라 사용자가 주석을 누를 때 표시 되는 설명선 이미지를 포함 하는 합니다.


Map 키트는 지도에 주석을 추가 하려면 iOS 위임 패턴을 사용 위치를 `Delegate` 의 속성을 `MKMapView` 의 인스턴스로 설정 됩니다는 `MKMapViewDelegate`. 이 대리자의이 구현을 반환 하는 일을 담당 하는 것은 `MKAnnotationView` 주석에 대 한 합니다.

주석의 추가 하려면 먼저 주석이 추가 됩니다 호출 하 여 `AddAnnotations` 에 `MKMapView` 인스턴스:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

주석의 위치를 지도에 표시 되 면 합니다 `MKMapView` 해당 대리자의 호출 `GetViewForAnnotation` 메서드를는 `MKAnnotationView` 표시할 합니다.

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

### <a name="reusing-annotations"></a>재사용 주석

메모리를 절약 하기 위해 `MKMapView` 주석 보기의 재사용을 표 셀 재사용 되는 방식과 유사 하 게 하기 위해 풀링할 수 있습니다. 호출 하 여 수행 됩니다 풀에서 주석 보기를 가져오는 `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>설명선이 포함 된 표시

앞에서 설명한 대로 주석을 설명선을 선택적으로 표시할 수 있습니다. 단순히 설명선을 표시 하도록 설정 `CanShowCallout` 에서 true로는 `MKAnnotationView`합니다. 이 인해 표시 된 것 처럼 주석 변하는데 표시 되는 주석의 제목:

 ![](images/04-callout.png "주석을 제목 표시")

### <a name="customizing-the-callout"></a>설명선을 사용자 지정

설명선도 사용자 지정할 수 있습니다 왼쪽 및 오른쪽 액세서리 뷰를 표시 하려면 아래와 같이:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

이 코드는 다음 설명선에 발생합니다.

 ![](images/05-callout-accessories.png "예제 설명선")

를 처리 하기 위해 탭 오른쪽 접근자를 구현 하기만 하면 됩니다 합니다 `CalloutAccessoryControlTapped` 의 메서드는 `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>오버레이

또 다른 방법은 지도에 계층 그래픽 오버레이 사용 중입니다. 오버레이는 확대/축소하는 대로 맵을 크기 조정하는 그래픽 그리기 콘텐츠를 지원합니다. iOS의 오버레이 비롯 한 여러 형식에 대 한 지원을 제공 합니다.

-  다각형-는 지도에 일부 영역을 강조 표시 하는 데 주로 사용 합니다.
-  다중선-경로 표시 하는 경우 주로 표시 됩니다.
-  원-맵의 순환 영역 강조 표시 하는 데 사용 합니다.


또한 세분화 된, 사용자 지정 그리기 코드를 사용 하 여 임의의 기 하 도형을 표시 하도록 사용자 지정 오버레이 만들 수 있습니다. 예를 들어 날씨 방사형 적합 한 사용자 지정 오버레이 대 한 것입니다.

#### <a name="adding-an-overlay"></a>오버레이 추가합니다.

주석 마찬가지로 오버레이 추가 다음과 같습니다. 2 부를

-  오버레이 대 한 모델 개체를 만들고 추가 하 여 `MKMapView` 입니다.
-  오버레이 대 한 보기를 만드는 `MKMapViewDelegate` 합니다.


오버레이 대 한 일 수 있습니다 `MKShape` 하위 클래스입니다. Xamarin.iOS 포함 `MKShape` 다각형, 다중선 및 원, 서브 클래스를 통해 합니다 `MKPolygon`, `MKPolyline` 및 `MKCircle` 각각 클래스입니다.

다음 코드를 추가 하는 예를 들어는 `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

오버레이 대 한 뷰를는 `MKOverlayView` 에서 반환 되는 인스턴스를 `GetViewForOverlay` 에 `MKMapViewDelegate`합니다. 각 `MKShape` 에 해당 `MKOverlayView` 지정된 된 도형 표시 하는 방법을 알고 있는 합니다. 에 대 한 `MKPolygon` 있기 `MKPolygonView`합니다. 마찬가지로 `MKPolyline` 에 해당 `MKPolylineView`, 및 `MKCircle` 있기 `MKCircleView`합니다.

예를 들어, 다음 코드 반환을 `MKCircleView` 에 대 한는 `MKCircle`:

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

이 표시 된 것 처럼 지도에 원을 표시 합니다.

 ![](images/06-circle-overlay.png "맵에 표시 된 원")

## <a name="local-search"></a>로컬 검색

iOS는 API는 지정 된 지리적 지역에 있는 관심 지점에 대 한 비동기 검색을 허용 하는 Map 키트를 사용 하 여 로컬 검색을 포함 합니다.

로컬 검색을 수행 하려면 응용 프로그램이 단계를 수행 해야 합니다.

1.  만들 `MKLocalSearchRequest` 개체입니다.
1.  만들기는 `MKLocalSearch` 에서 개체를 `MKLocalSearchRequest` 입니다.
1.  호출 된 `Start` 메서드는 `MKLocalSearch` 개체입니다.
1.  검색 된 `MKLocalSearchResponse` 콜백에서 개체입니다.


로컬 검색 API 자체는 사용자 인터페이스를 제공합니다. 사용할 맵도 않아도 됩니다. 그러나 로컬 검색으로 사용 하도록 응용 프로그램 검색 쿼리를 지정 하 고 결과 표시 하기 위한 몇 가지 방법을 제공 해야 합니다. 또한 결과 위치 데이터를 포함할 수 있으므로 종종 합리적일 지도에 표시할.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>로컬 검색 UI 추가

검색 입력을 허용 하는 한 가지 방법은 된를 `UISearchBar`에서 제공 하는 한 `UISearchController` 결과 테이블에 표시 됩니다.

다음 코드를 추가 합니다 `UISearchController` (있는 검색 표시줄 속성)에 `ViewDidLoad` 메서드의 `MapViewController`:

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
```

사용자 인터페이스를 검색 모음 개체를 통합 하는 것에 대 한 책임이 참고 합니다. 이 예제에서는 탐색 모음의 TitleView 할당에서는 있지만 표시할 수 있는 다른 위치를 찾을 수는 응용 프로그램에서 탐색 컨트롤러를 사용 하지 않는 경우.

이 코드 조각에서는 다른 사용자 지정 뷰 컨트롤러 – 만들었습니다 `searchResultsController` – 검색 결과 표시 하 고 다음은 검색 컨트롤러 개체를 만들려면이 개체를 사용 했습니다. 검색 표시줄을 사용 하 여 상호 작용할 때 활성화 되는 새 검색 업데이트를 만들었습니다. 각 키 입력을 사용 하 여 검색 하는 방법에 대 한 알림을 수신 하 고 UI를 업데이트 해야 합니다.
둘 다 구현 하는 방법을 살펴보는 됩니다 것를 `searchResultsController` 하며 `searchResultsUpdater` 이 가이드의 뒷부분에 나오는.

이 인해 검색 표시줄 아래와 같이 지도 표시:

 ![](images/07-searchbar.png "지도 표시를 검색 창")
 


### <a name="displaying-the-search-results"></a>검색 결과 표시합니다.

검색 결과 표시 하려면 사용자 지정 뷰 컨트롤러를;을 생성 해야 일반적으로 `UITableViewController`합니다. 위에 나와 있는 것 처럼 합니다 `searchResultsController` 의 생성자에 전달 되는 `searchController` 만들 때.
다음 코드는이 사용자 지정 뷰 컨트롤러를 만드는 방법의 예:

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

### <a name="updating-the-search-results"></a>검색 결과 업데이트 하는 중

합니다 `SearchResultsUpdater` 사이의 중개자 역할을 `searchController`의 검색 표시줄 및 검색 결과입니다. 

이 예제에서는 먼저 만들어야 할의 검색 메서드에 `SearchResultsViewController`합니다. 만들어야이 작업을 수행 하는 `MKLocalSearch` 개체를 검색을 실행 하는 데 사용를 `MKLocalSearchRequest`에 전달 된 콜백에서 결과가 검색 되는 `Start` 메서드의 `MKLocalSearch` 개체. 그런 다음 결과가 반환 되는 `MKLocalSearchResponse` 개체의 배열이 포함 된 `MKMapItem` 개체:

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

그런 다음 우리의 `MapViewController` 의 사용자 지정 구현을 만들겠습니다 `UISearchResultsUpdating`에 할당 되는 `SearchResultsUpdater` 의 속성 우리의 `searchController` 에 [로컬 검색 UI를 추가](#Adding_a_Local_Search_UI) 섹션:

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

위의 구현 주석의를 맵에 추가 결과에서 항목을 선택 하는 경우 아래와 같이:

 ![](images/08-search-results.png "결과에서 항목을 선택 하는 경우 지도에 추가 하는 주석")
 
> [!IMPORTANT]
> `UISearchController` iOS 8 구현 되었습니다. 따라서 이전의 장치를 지원 하려는 경우 사용 해야 `UISearchDisplayController`합니다.



## <a name="summary"></a>요약

검사 하는이 문서는 *지도* *키트* iOS 프레임 워크입니다. 첫째,이 하는 방법을 확인할 수는 `MKMapView` 클래스를 사용 하면 응용 프로그램에 포함 하는 대화형 매핑됩니다. 그런 다음 추가 주석 및 오버레이 사용 하 여 맵을 사용자 지정 하는 방법을 보여 줍니다. 마지막으로 조사 하 고 ios 6.1, Map 키트에 추가 된 로컬 검색 기능 사용 방법을 보여 주는 관심 지점에 대 한 위치 기반 쿼리를 수행 및 지도에 추가 합니다.

## <a name="related-links"></a>관련 링크

- [SearchController](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [MapDemo (샘플)](https://developer.xamarin.com/samples/monotouch/MapDemo)
