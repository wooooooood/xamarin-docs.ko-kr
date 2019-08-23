---
title: Xamarin.ios의 Maps
description: 이 문서에서는 iOS MapKit 프레임 워크 및이를 Xamarin.ios와 함께 사용 하는 방법을 설명 합니다. 지도를 추가 하 고, 스타일을 적용 하 고, 이동 및 확대/축소 하 고, 사용자 위치를 사용 하 고, 주석을 추가 하 고, 콜아웃 및 오버레이를 사용 하 고, 로컬 검색을 수행 하는
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4b0e540bdcdf061f64880ea961a5e07a0a45b22e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68642908"
---
# <a name="maps-in-xamarinios"></a>Xamarin.ios의 Maps

맵은 모든 최신 모바일 운영 체제에서 일반적으로 사용할 수 있는 기능입니다. iOS는 맵 키트 프레임 워크를 통해 기본적으로 매핑 지원을 제공 합니다. 지도 키트를 사용 하면 응용 프로그램에서 풍부한 대화형 지도를 쉽게 추가할 수 있습니다. 이러한 맵은 지도의 위치를 표시 하는 주석을 추가 하 고 임의 모양의 그래픽을 표시 하는 등의 다양 한 방법으로 사용자 지정할 수 있습니다. 지도 키트에는 장치의 현재 위치를 표시 하는 기본 지원도 있습니다.

## <a name="adding-a-map"></a>지도 추가

응용 프로그램에 맵을 추가 하려면 아래와 같이 뷰 계층 `MKMapView` 구조에 인스턴스를 추가 합니다.

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView`는 맵을 표시 하는 하위클래스입니다.`UIView` 위의 코드를 사용 하 여 지도를 추가 하기만 하면 대화형 지도가 생성 됩니다.

 ![](images/00-map.png "샘플 맵")

## <a name="map-style"></a>지도 스타일

 `MKMapView`3 가지 지도 스타일을 지원 합니다. 지도 스타일을 적용 하려면 `MapType` 속성을 `MKMapType` 열거형의 값으로 설정 하면 됩니다.
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  다음 스크린샷에서는 사용할 수 있는 다양 한 지도 스타일을 보여 줍니다.

 ![](images/01-mapstyles.png "이 스크린샷에서는 사용 가능한 다양 한 지도 스타일을 보여 줍니다.")

## <a name="panning-and-zooming"></a>패닝 및 확대/축소

 `MKMapView`에는 다음과 같은 지도 상호 작용 기능에 대 한 지원이 포함 되어 있습니다.

-  손가락을 사용한 확대/축소 제스처
-  이동 제스처를 통해 패닝


`ZoomEnabled` 인스턴스의 및`ScrollEnabled` 속성을 설정 하 여 이러한 기능을 사용 하거나 사용 하지 않도록 설정할 수 있습니다. 여기서 기본값은 두 경우 모두 true입니다. `MKMapView` 예를 들어 정적 맵을 표시 하려면 적절 한 속성을 false로 설정 하면 됩니다.

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>사용자 위치

사용자 상호 작용 외에도 `MKMapView` 에서는 장치 위치를 기본적으로 지원 합니다. *핵심 위치* 프레임 워크를 사용 하 여이를 수행 합니다. 사용자의 위치에 액세스 하기 전에 사용자에 게 메시지를 표시 해야 합니다. 이렇게 하려면의 `CLLocationManager` 인스턴스를 만들고를 호출 `RequestWhenInUseAuthorization`합니다.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

8\.0 이전의 iOS 버전에서를 호출 `RequestWhenInUseAuthorization` 하려고 하면 오류가 발생 합니다. 8 이전 버전을 지원 하려는 경우 해당 통화를 수행 하기 전에 iOS의 버전을 확인 해야 합니다.

사용자의 위치에 액세스 하려면 **info.plist**도 수정 해야 합니다. 위치 데이터와 관련된 다음 키를 설정해야 합니다.

- **NSLocationWhenInUseUsageDescription** - 사용자가 앱과 상호 작용하는 동안 사용자의 위치에 액세스하는 경우
- **NSLocationAlwaysUsageDescription** - 앱이 백그라운드에서 사용자의 위치에 액세스하는 경우

**Info.plist** 를 열고 편집기의 맨 아래에 있는 *원본* 을 선택 하 여 해당 키를 추가할 수 있습니다.

**Info.plist** 를 업데이트 하 고 사용자에 게 해당 위치에 액세스할 수 있는 권한을 요청 하 라는 메시지를 표시 한 후에는 속성을 `ShowsUserLocation` true로 설정 하 여 맵에 사용자의 위치를 표시할 수 있습니다.

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "위치 액세스 허용 경고")
 
## <a name="annotations"></a>주석

 `MKMapView`는 지도에서 주석 이라고 하는 이미지 표시도 지원 합니다. 사용자 지정 이미지 또는 다양 한 색의 시스템 정의 핀 중 하나일 수 있습니다. 예를 들어 다음 스크린샷은 핀과 사용자 지정 이미지가 모두 포함 된 맵을 보여 줍니다.

 ![](images/03-annotations.png "이 스크린샷에서는 핀과 사용자 지정 이미지가 모두 포함 된 맵을 보여 줍니다.")

### <a name="adding-an-annotation"></a>주석 추가

주석 자체는 다음과 같은 두 부분으로 구성 됩니다.

-  주석의 제목 및 위치와 같은 주석에 대 한 모델 데이터를 포함 하는 개체입니다.`MKAnnotation`
-  `MKAnnotationView` 표시할 이미지를 포함 하는와 사용자가 주석을 누를 때 표시 되는 선택적 설명선입니다.


지도 키트는 iOS 위임 패턴을 사용 하 여 맵에 주석을 추가 합니다 `Delegate` `MKMapViewDelegate`. 여기서 `MKMapView` 의 속성은의 인스턴스로 설정 됩니다. 주석에 대 한를 `MKAnnotationView` 반환 하는이 대리자의 구현입니다.

주석을 추가 하려면 먼저 `AddAnnotations` `MKMapView` 인스턴스에 대해를 호출 하 여 주석을 추가 합니다.

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

맵의 위치가 맵에 표시 되 면는 `MKMapView` 해당 대리자의 `GetViewForAnnotation` 메서드를 호출 하 `MKAnnotationView` 여를 표시 합니다.

예를 들어 다음 코드는 시스템에서 제공 `MKPinAnnotationView`하는를 반환 합니다.

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

### <a name="reusing-annotations"></a>주석 재사용

메모리를 절약 하기 `MKMapView` 위해에서 테이블 셀을 다시 사용 하는 방식과 비슷하게 주석 보기를 다시 사용 하기 위해 풀링할 수 있습니다. 풀에서 주석 뷰 가져오기는를 `DequeueReusableAnnotation`호출 하 여 수행 됩니다.

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>설명선 표시

앞서 언급 했 듯이 주석은 선택적으로 설명선을 표시할 수 있습니다. 설명선을 표시 하려면에 `CanShowCallout` `MKAnnotationView`대해 간단히 true로 설정 합니다. 이렇게 하면 다음과 같이 주석을 누를 때 주석의 제목이 표시 됩니다.

 ![](images/04-callout.png "표시 되는 주석 제목입니다.")

### <a name="customizing-the-callout"></a>설명선 사용자 지정

아래와 같이 왼쪽 및 오른쪽 액세서리 보기를 표시 하도록 설명선을 사용자 지정할 수도 있습니다.

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

이 코드는 다음 설명선을 생성 합니다.

 ![](images/05-callout-accessories.png "예제 설명선")

오른쪽 액세서리를 누르는 사용자를 처리 하려면 `CalloutAccessoryControlTapped` `MKMapViewDelegate`에서 메서드를 구현 하기만 하면 됩니다.

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>오버레이

지도에서 그래픽을 계층화 하는 또 다른 방법은 오버레이를 사용 하는 것입니다. 오버레이는 확대/축소하는 대로 맵을 크기 조정하는 그래픽 그리기 콘텐츠를 지원합니다. iOS는 다음과 같은 몇 가지 유형의 오버레이를 지원 합니다.

-  다각형-지도에서 일부 영역을 강조 표시 하는 데 주로 사용 됩니다.
-  다중선-경로를 표시할 때 주로 표시 됩니다.
-  원-지도의 원형 영역을 강조 표시 하는 데 사용 됩니다.


또한 세부적인 사용자 지정 그리기 코드를 사용 하 여 임의 기 하 도형을 표시 하도록 사용자 지정 오버레이를 만들 수 있습니다. 예를 들어 날씨 레이더는 사용자 지정 오버레이의 좋은 후보가 됩니다.

#### <a name="adding-an-overlay"></a>오버레이 추가

주석과 유사 하 게 오버레이를 추가 하는 과정은 다음 두 부분으로 구성 됩니다.

-  오버레이의 모델 개체를 만들어에 추가 `MKMapView` 합니다.
-  에서 오버레이의 보기를 만듭니다 `MKMapViewDelegate` .


오버레이에 대 한 모델은 모든 `MKShape` 하위 클래스일 수 있습니다. Xamarin.ios에는 `MKPolygon`, `MKShape` `MKPolyline` 및`MKCircle` 클래스를 통해 다각형, 폴리라인 및 원의 서브 클래스가 포함 됩니다.

예를 들어 다음 코드는를 추가 `MKCircle`하는 데 사용 됩니다.

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

오버레이 `MKOverlayView` 에 대 한 뷰는의에서 반환 `GetViewForOverlay` `MKMapViewDelegate`하는 인스턴스입니다. 각 `MKShape` 에는 지정 `MKOverlayView` 된 모양을 표시 하는 방법을 알고 있는가 있습니다. 에 `MKPolygon` 는가 `MKPolygonView`있습니다. 마찬가지로, `MKPolyline` 은에 `MKPolylineView`해당 하 고 `MKCircle` 는 `MKCircleView`에 해당 합니다.

예를 들어 다음 코드는 `MKCircleView` `MKCircle`에 대 한를 반환 합니다.

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

그러면 다음과 같이 맵에 원이 표시 됩니다.

 ![](images/06-circle-overlay.png "지도에 표시 되는 원")

## <a name="local-search"></a>로컬 검색

iOS에는 지정 된 지역에서 관심 지점의 비동기 검색을 허용 하는 맵 키트를 사용 하는 로컬 검색 API가 포함 되어 있습니다.

로컬 검색을 수행 하려면 응용 프로그램에서 다음 단계를 수행 해야 합니다.

1.  개체 `MKLocalSearchRequest` 를 만듭니다.
1.  에서 개체를 만듭니다. `MKLocalSearchRequest` `MKLocalSearch`
1.  개체에서 메서드를 `Start` 호출 합니다. `MKLocalSearch`
1.  콜백에서 개체 `MKLocalSearchResponse` 를 검색 합니다.


로컬 검색 API 자체는 사용자 인터페이스를 제공 하지 않습니다. 지도를 사용 하지 않아도 됩니다. 그러나 로컬 검색을 효과적으로 사용 하려면 응용 프로그램에서 검색 쿼리를 지정 하 고 결과를 표시 하는 몇 가지 방법을 제공 해야 합니다. 또한 결과에 위치 데이터가 포함 되므로 맵에 표시 하는 것이 좋습니다.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>로컬 검색 UI 추가

검색 입력을 허용 하는 한 가지 방법은 `UISearchBar`에서 제공 `UISearchController` 하는를 사용 하는 것 이며 테이블에 결과를 표시 합니다.

다음 코드에서는의 `UISearchController` `ViewDidLoad` `MapViewController`메서드에 검색 표시줄 속성이 있는를 추가 합니다.

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

사용자 인터페이스에 검색 표시줄 개체를 통합 하는 일을 담당 합니다. 이 예제에서는 탐색 모음의 TitleView에 할당 했습니다. 그러나 응용 프로그램에서 탐색 컨트롤러를 사용 하지 않는 경우에는 표시할 다른 장소를 찾아야 합니다.

이 코드 조각에서는 검색 결과를 표시 하 고이 개체 `searchResultsController` 를 사용 하 여 검색 컨트롤러 개체를 만든 다른 사용자 지정 뷰 컨트롤러를 만들었습니다. 또한 사용자가 검색 표시줄과 상호 작용할 때 활성화 되는 새 검색 업데이트를 만들었습니다. 각 키 입력이 있는 검색에 대 한 알림을 받고 UI 업데이트를 담당 합니다.
이 가이드의 `searchResultsController` `searchResultsUpdater` 뒷부분에 나오는 및를 모두 구현 하는 방법을 살펴보겠습니다.

그러면 아래와 같이 지도 위에 검색 표시줄이 표시 됩니다.

 ![](images/07-searchbar.png "지도 위에 표시 되는 검색 표시줄")
 


### <a name="displaying-the-search-results"></a>검색 결과 표시

검색 결과를 표시 하려면 사용자 지정 뷰 컨트롤러를 만들어야 합니다. 일반적으로 `UITableViewController`입니다. 위와 `searchResultsController` 같이는 생성 `searchController` 될 때의 생성자에 전달 됩니다.
다음 코드는이 사용자 지정 뷰 컨트롤러를 만드는 방법의 예입니다.

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

는 `SearchResultsUpdater` `searchController`의 검색 표시줄과 검색 결과 간의 mediator 역할을 합니다. 

이 예제에서는 먼저에서 `SearchResultsViewController`검색 메서드를 만들어야 합니다. 이렇게 하려면 `MKLocalSearch` 개체를 만들고이 개체를 사용 하 여 `MKLocalSearchRequest`에 대 한 검색을 실행 해야 합니다. 그러면 `MKLocalSearch` 개체의 `Start` 메서드에 전달 된 콜백에서 결과가 검색 됩니다. 그런 다음 개체의 `MKLocalSearchResponse` `MKMapItem` 배열을 포함 하는 개체에서 결과가 반환 됩니다.

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

그런다음`MapViewController`가 `UISearchResultsUpdating`의 사용자 지정 구현을 만듭니다 .이 `searchController`구현은 [로컬 검색 UI 추가 섹션](#Adding_a_Local_Search_UI)에서의 `SearchResultsUpdater`속성에 할당 됩니다.

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

위의 구현은 아래와 같이 결과에서 항목을 선택할 때 맵에 주석을 추가 합니다.

 ![](images/08-search-results.png "결과에서 항목을 선택할 때 맵에 추가 되는 주석입니다.")
 
> [!IMPORTANT]
> `UISearchController`는 iOS 8에서 구현 되었습니다. 이전 장치를 지원 하려는 경우를 사용 `UISearchDisplayController`해야 합니다.



## <a name="summary"></a>요약

이 문서에서는 iOS 용 *맵* *키트* 프레임 워크를 살펴보았습니다. 먼저 `MKMapView` 클래스가 응용 프로그램에 대화형 맵을 포함 하도록 허용 하는 방법을 살펴보았습니다. 그런 다음 주석과 오버레이를 사용 하 여 지도를 추가로 사용자 지정 하는 방법을 보여 주었습니다. 마지막으로, 대상 위치에 대해 위치 기반 쿼리를 수행 하 고 지도에 추가 하는 방법을 보여 주는 iOS 6.1를 사용 하 여 Map Kit에 추가 된 로컬 검색 기능을 검사 했습니다.

## <a name="related-links"></a>관련 링크

- [SearchController](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [MapDemo (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/mapdemo)
