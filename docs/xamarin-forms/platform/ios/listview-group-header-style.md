---
제목: "iOS의 ListView 그룹 헤더 스타일" 설명: "플랫폼별를 사용 하면 사용자 지정 렌더러 나 효과를 구현 하지 않고 특정 플랫폼 에서만 사용할 수 있는 기능을 사용할 수 있습니다. 이 문서에서는 스크롤 하는 동안 ListView 머리글 셀의 부동 여부를 제어 하는 iOS 플랫폼별를 사용 하는 방법을 설명 합니다. "
assetid: 099B2C7F-727F-4BCF-903B-87E728108C24 밀리초. 기술: xamarin forms author: davidbritch: dabritch: ms. date: 10/24/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="listview-group-header-style-on-ios"></a>IOS의 ListView 그룹 머리글 스타일

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

이 iOS 플랫폼별 [`ListView`](xref:Xamarin.Forms.ListView) 는 스크롤 하는 동안 머리글 셀이 부동 되는지 여부를 제어 합니다. `ListView.GroupHeaderStyle`바인딩 가능한 속성을 열거형의 값으로 설정 하 여 XAML에서 사용 됩니다 `GroupHeaderStyle` .

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.GroupHeaderStyle="Grouped">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

또는 흐름 API를 사용 하 여 c #에서 사용할 수 있습니다.

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

`ListView.On<iOS>`메서드는이 플랫폼별가 iOS 에서만 실행 되도록 지정 합니다. `ListView.SetGroupHeaderStyle`네임 스페이스의 메서드는 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) [`ListView`](xref:Xamarin.Forms.ListView) 스크롤 하는 동안 머리글 셀의 부동 여부를 제어 하는 데 사용 됩니다. `GroupHeaderStyle`열거형은 다음 두 가지 가능한 값을 제공 합니다.

- `Plain`–가 스크롤 될 때 머리글 셀이 float을 나타냅니다 [`ListView`](xref:Xamarin.Forms.ListView) (기본값).
- `Grouped`–가 스크롤될 때 머리글 셀이 부동 되지 않음을 나타냅니다 [`ListView`](xref:Xamarin.Forms.ListView) .

또한 메서드를 사용 하 여에 `ListView.GetGroupHeaderStyle` 적용 되는를 반환할 수 있습니다 `GroupHeaderStyle` [`ListView`](xref:Xamarin.Forms.ListView) .

그러면 지정 된 `GroupHeaderStyle` 값이에 적용 되어 [`ListView`](xref:Xamarin.Forms.ListView) 스크롤 중에 머리글 셀의 float 여부를 제어 합니다.

[![IOS의 부동 및 비 부동 ListView 머리글 셀의 스크린샷](listview-group-header-style-images/group-header-styles.png "부동 및 비 부동 머리글 셀이 있는 ListView")](listview-group-header-style-images/group-header-styles-large.png#lightbox "부동 및 비 부동 머리글 셀이 있는 ListView")

## <a name="related-links"></a>관련 링크

- [PlatformSpecifics (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [플랫폼별 만들기](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
