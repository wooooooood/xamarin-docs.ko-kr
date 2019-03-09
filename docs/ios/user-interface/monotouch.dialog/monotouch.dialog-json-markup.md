---
title: MonoTouch.Dialog Json 태그
description: 이 문서에서는 MonoTouch.Dialog를 사용 하 여 Xamarin.iOS 사용자 인터페이스를 구축 하는 JSON 구문을 설명 합니다.
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: 8edabfc6fa3988af0dd38dbfd9daeb1c4003c33e
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670768"
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog Json 태그

이 페이지에서는 MonoTouch.Dialog의에서 허용 하는 Json 태그 [JsonElement](https://developer.xamarin.com/api/type/MonoTouch.Dialog.JsonElement/)

예를 들어 시작 하겠습니다. 다음은 JsonElement에 전달할 수 있는 완전 한 Json 파일입니다.

```csharp
{     
  "title": "Json Sample",
  "sections": [ 
      {
          "header": "Booleans",
          "footer": "Slider or image-based",
          "id": "first-section",
          "elements": [
              { 
                  "type" : "boolean",
                  "caption" : "Demo of a Boolean",
                  "value"   : true
              }, {
                  "type": "boolean",
                  "caption" : "Boolean using images",
                  "value"   : false,
                  "on"      : "favorite.png",
                  "off"     : "~/favorited.png"
              }, {
                      "type": "root",
                      "title": "Tap for nested controller",
                      "sections": [ {
                         "header": "Nested view!",
                         "elements": [
                           {
                             "type": "boolean",
                             "caption": "Just a boolean",
                             "id": "the-boolean",
                             "value": false
                           },
                           {
                             "type": "string",
                             "caption": "Welcome to the nested controller"
                           }
                         ]
                       }
                     ]
                   }
          ]
      }, {
          "header": "Entries",
          "elements" : [
              {
                  "type": "entry",
                  "caption": "Username",
                  "value": "",
                  "placeholder": "Your account username"
              }
          ]
      }
  ]
}
```

위의 태그는 다음 UI를 생성합니다.

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "지정 된 태그에서 만든 UI")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

트리의 모든 요소에는 속성이 포함 될 수 있습니다 `"id"`합니다. 런타임 시 수는 개별 섹션 또는 JsonElement 인덱서를 사용 하 여 요소를 참조 합니다. 다음과 같습니다.

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement
```

 <a name="Root_Element_Syntax" />


## <a name="root-element-syntax"></a>루트 요소 구문

루트 요소는 다음 값을 포함 합니다.

-  `title`
-  `sections` (선택 사항)


루트 요소는 섹션 내에서 중첩 된 컨트롤러를 만들려면 요소로 나타날 수 있습니다. 이런 경우, 추가 속성 `"type"` 로 설정 되어야 합니다 `"root"`

 <a name="url" />


### <a name="url"></a>url

경우는 `"url"` 속성이 설정 되어 사용자 탭이 RootElement에서 코드가 지정된 된 url에서 파일을 요청 하 고 내용을 표시 되는 새 정보를 확인 합니다. 만드는 데 사용할 수 있습니다 사용자가 탭 기반 서버에서 사용자 인터페이스를 확장 합니다.

 <a name="group" />


### <a name="group"></a>그룹

경우 설정 집합이 루트 요소에 대 한 그룹 이름입니다. 요소에 중첩 된 요소 중 하나에서 루트 요소의 값으로 표시 되는 요약 선택 그룹 이름이 사용 됩니다. 확인란의 값 또는 라디오 단추의 값 중 하나입니다.

 <a name="radioselected" />


### <a name="radioselected"></a>radioselected

중첩 된 요소에서 선택 된 라디오 항목 식별

 <a name="title" />


### <a name="title"></a>제목

RootElement에 사용 되는 제목이 있는 경우 됩니다.

 <a name="type" />


### <a name="type"></a>type

로 설정 되어야 합니다 `"root"` 때 (컨트롤러 중첩 사용 됨) 섹션에 표시 됩니다.

 <a name="sections" />


### <a name="sections"></a>섹션

이 개별 섹션을 사용 하 여 Json 배열

 <a name="Section_Syntax" />


## <a name="section-syntax"></a>섹션 구문

섹션에는 다음이 포함 됩니다.

-  `header` (선택 사항)
-  `footer` (선택 사항)
-  `elements` 배열


 <a name="header" />


### <a name="header"></a>헤더

있는 경우 머리글 텍스트 섹션의 캡션으로 표시 됩니다.

 <a name="footer" />


### <a name="footer"></a>바닥글

있는 경우 바닥글 섹션의 맨 아래에 표시 됩니다.

 <a name="elements" />


### <a name="elements"></a>요소

요소의 배열입니다. 각 요소에는 하나 이상의 키를 포함 해야 합니다 `"type"` 만들 요소의 종류를 식별 하는 데 사용 되는 키입니다.
와 같은 일부 일반적인 속성을 공유 요소 중 일부가 `"caption"` 고 `"value"`입니다. 다음은 지원 되는 요소의 목록입니다.

-  `string` (모두와 스타일 지정 없는) 요소
-  `entry` 줄 (일반 또는 암호)
-  `boolean` 값 (스위치 또는 이미지 사용)


문자열 요소 수 단추와 메서드를 제공 하 여 사용자가 셀 또는 접근자를 누를 때 호출할

 <a name="Rendering_Elements" />


## <a name="rendering-elements"></a>요소를 렌더링합니다.

렌더링 요소를 기반으로 C# StringElement StyledStringElement 않으며 다양 한 방법으로 정보를 렌더링할 수 및 다양 한 방법으로를 렌더링 하는 것이 가능 합니다. 다음과 같은 간단한 요소를 만들 수 있습니다.

```csharp
{
        "type": "string",
        "caption": "Json Serializer",
}
```

모든 기본값을 사용 하 여 간단한 문자열로 표시 됩니다: 글꼴, 배경, 텍스트 색 및 장식 합니다. 이러한 요소에 작업 연결을 설정 하 여 단추 처럼 동작할 수 있도록 하는 것이 가능 합니다 `"ontap"` 속성 또는 `"onaccessorytap"` 속성:

```csharp
{
    "type":    "string",
        "caption": "View Photos",
        "ontap:    "Acme.PhotoLibrary.ShowPhotos"
}
```

위의 "Acme.PhotoLibrary" 클래스에서 "ShowPhotos" 메서드를 호출 합니다. `"onaccessorytap"` 는 유사 하지만 사용자가 셀을 탭 하는 대신 액세서리를 누를 경우에 호출 됩니다. 이 기능을 사용 하려면 또한 접근자를 설정 해야 합니다.

```csharp
{
    "type":     "string",
        "caption":  "View Photos",
        "ontap:     "Acme.PhotoLibrary.ShowPhotos",
        "accessory: "detail-disclosure",
        "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

요소 렌더링 두 문자열을 한 번에 표시할 수 있습니다, 하나는 캡션이 이며 다른 값입니다. 스타일에 따라 달라 집니다을 사용 하 여 설정할 수 있습니다 이러한 문자열은 렌더링 하는 방법의 `"style"` 속성입니다. 기본 왼쪽 및 오른쪽에 있는 값에 캡션이 표시 됩니다. 자세한 스타일에는 섹션을 참조 하세요. 색은 빨간색, 녹색, 파란색 및 아마도 알파 값에 대 한 값을 나타내는 16 진수 숫자가 뒤에 '#' 기호를 사용 하 여 인코딩됩니다. RGBA 또는 RGB 값을 나타내는 약식 표현 (자리 16 진수 3 또는 4)에서 콘텐츠를 인코딩할 수 있습니다. 또는 RGB 또는 RGBA 값 표시 하는 긴 형식 (6 개월 또는 8 자리). 짧은 버전은 동일한 16 진수를 두 번 작성 하는 줄임. "#1bc" 상수에 빨간색으로 좋아요 이므로 0x11, 녹색 = = 0xbb 파란색 0xcc =. 알파 값이 없는 경우에 색이 불투명 합니다. 몇 가지 예:

```csharp
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />


### <a name="accessory"></a>액세서리

가능한 값은 렌더링 요소에 표시 될 액세서리의 종류를 결정 합니다.

-  `checkmark`
-  `detail-disclosure`
-  `disclosure-indicator`


값이 없는 경우에 없는 액세서리 표시 됩니다.

 <a name="background" />


### <a name="background"></a>배경

Background 속성 셀에 대 한 배경색을 설정 합니다. 값이 중 하나는 이미지의 URL입니다 (이 경우 비동기 이미지 다운로더 호출할 및 배경 이미지 다운로드 되 면 업데이트 됩니다) 색 구문을 사용 하 여 지정 된 색이 될 수도 있습니다.

 <a name="caption" />


### <a name="caption"></a>캡션

렌더링 요소에 표시할 기본 문자열입니다. 글꼴 및 색을 설정 하 여 지정할 수는 `"textcolor"` 고 `"font"` 속성입니다. 렌더링 스타일에 의해 결정 됩니다는 `"style"` 속성입니다.

 <a name="color_and_detailcolor" />


### <a name="color-and-detailcolor"></a>색 및 detailcolor

자세한 텍스트 또는 본문에 사용할 색입니다.

 <a name="detailfont_and_font" />


### <a name="detailfont-and-font"></a>detailfont 및 글꼴

캡션 또는 정보 텍스트에 사용할 글꼴입니다. 글꼴을 지정 하는 형식에는 대시 및 포인트 크기를 필요에 따라 뒤 글꼴 이름입니다.
다음은 올바른 글꼴 사양입니다.

-  "Helvetica"
-  "Helvetica-14"


 <a name="linebreak" />


### <a name="linebreak"></a>linebreak

선 분할 하는 방법을 결정 합니다. 가능한 값은 다음과 같습니다.

-  `character-wrap`
-  `clip`
-  `head-truncation`
-  `middle-truncation`
-  `tail-truncation`
-  `word-wrap`


둘 다 `character-wrap` 및 `word-wrap` 와 함께 사용할 수는 `"lines"` 속성 집합을 0으로 여러 줄 요소로 렌더링 요소를 설정 합니다.

 <a name="ontap_and_onaccessorytap" />


### <a name="ontap-and-onaccessorytap"></a>ontap 및 onaccessorytap

이러한 속성을 매개 변수로 개체를 사용 하는 응용 프로그램에 정적 메서드 이름을 가리켜야 합니다. JsonDialog.FromFile 또는 JsonDialog.FromJson 메서드를 사용 하 여 계층을 만들 때에 선택적 개체 값을 전달할 수 있습니다. 그러면이 개체 값을 메서드에 전달 됩니다. 정적 메서드를 일부 컨텍스트를 전달 합니다.이 사용할 수 있습니다. 예를 들어:

```csharp
class Foo {
    Foo ()
    {
        root = JsonDialog.FromJson (myJson, this);
    }

    static void Callback (object obj)
    {
        Foo myFoo = (Foo) obj;
        obj.Callback ();
    }
}
```

 <a name="lines" />


### <a name="lines"></a>선

이 0으로 설정 되 고 포함 된 문자열의 내용에 따라 요소 자동 크기 생성 됩니다. 이렇게 하려면 설정 해야 합니다 `"linebreak"` 속성을 `"character-wrap"` 또는 `"word-wrap"`합니다.

 <a name="style" />


### <a name="style"></a>스타일

콘텐츠를 렌더링 하는 데 사용할 셀 스타일의 종류를 결정 하는 스타일 및 UITableViewCellStyle 열거형 값에 해당 합니다.
가능한 값은 다음과 같습니다.

-  `"default"`
-  `"value1"`
-  `"value2"`
-  `"subtitle"` : 부제목의 텍스트입니다.


 <a name="subtitle" />


### <a name="subtitle"></a>부제목

부제목에 사용할 값입니다. 스타일을 설정 하려면 바로 가기 `"subtitle"` 설정의 `"value"` 속성 문자열을 합니다.
이 단일 항목을 사용 하 여 모두를 수행 합니다.

 <a name="textcolor" />


### <a name="textcolor"></a>textcolor

텍스트에 사용할 색입니다.

 <a name="value" />


### <a name="value"></a>값

렌더링 요소에 표시할 보조 값입니다. 이 레이아웃은 영향을 받지는 `"style"` 설정 합니다. 글꼴 및 색을 설정 하 여 지정할 수는 `"detailfont"` 고 `"detailcolor"`입니다.

 <a name="Boolean_Elements" />


## <a name="boolean-elements"></a>부울 요소

부울 요소 형식을 설정 해야 `"bool"`에 포함 될 수 있습니다를 `"caption"` 표시할 및 `"value"` true 또는 false로 설정 됩니다. 경우는 `"on"` 고 `"off"` 속성 설정, 이미지 수로 간주 됩니다. 이미지는 응용 프로그램에서 현재 작업 디렉터리를 기준으로 확인 됩니다. 번들에 상대적인 파일을 참조 하려는 경우 사용할 수 있습니다는 `"~"` 간단 하 게 응용 프로그램 번들 디렉터리를 나타냅니다. 예를 들어 `"~/favorite.png"` 번들 파일에 포함 된 favorite.png 됩니다. 예를 들어:

```csharp
{ 
    "type" : "boolean",
    "caption" : "Demo of a Boolean",
    "value"   : true
},

{
    "type": "boolean",
    "caption" : "Boolean using images",
    "value"   : false,
    "on"      : "favorite.png",
    "off"     : "~/favorited.png"
}
```

 <a name="type" />


### <a name="type"></a>type

형식 수로 설정할 수 `"boolean"` 또는 `"checkbox"`합니다. 부울으로 사용 합니다는 UISlider 또는 이미지 (둘 다 `"on"` 및 `"off"` 설정). 확인란으로 사용 합니다는 확인란을 선택 합니다. `"group"` 속성을 특정 그룹에 속하는 것으로 부울 요소 태그를 사용할 수 있습니다. 이 포함 된 루트에 있는 경우에 유용는 `"group"` 루트로 속성에서는 동일한 그룹에 속하는 모든 부울 (또는 확인란) 개수를 사용 하 여 결과 요약 합니다.

 <a name="Entry_Elements" />


## <a name="entry-elements"></a>항목 요소

항목 요소를 사용 하 여 사용자 데이터를 입력할 수 있도록 합니다. 항목 요소에 대 한 형식은 `"entry"` 또는 `"password"`합니다. 합니다 `"caption"` 오른쪽에 표시할 텍스트 속성은 및 `"value"` 의 항목 설정 하려면 초기 값으로 설정 됩니다. `"placeholder"` (그림 회색) 빈 항목에 대 한 사용자에 게 힌트를 표시 하는 데 사용 됩니다. 다음은 몇 가지 예입니다.

```csharp
{
        "type": "entry",
        "caption": "Username",
        "value": "",
        "placeholder": "Your account username"
}, {
        "type": "password",
        "caption": "Password",
        "value": "",
        "placeholder": "You password"
}, {
        "type": "entry",
        "caption": "Zip Code",
        "value": "01010",
        "placeholder": "your zip code",
        "keyboard": "numbers"
}, {
        "type": "entry",
        "return-key": "route",
        "caption": "Entry with 'route'",
        "placeholder": "captialization all + no corrections",
        "capitalization": "all",
        "autocorrect": "no"
}
```

 <a name="autocorrect" />


### <a name="autocorrect"></a>자동 고침

자동 수정 스타일 항목에 대 한 사용을 확인 합니다. 가능한 값은 true 또는 false (또는 문자열 `"yes"` 고 `"no"`).

 <a name="capitalization" />


### <a name="capitalization"></a>대/소문자

대/소문자 스타일 항목에 대 한 사용입니다. 가능한 값은 다음과 같습니다.

-  `all`
-  `none`
-  `sentences`
-  `words`


 <a name="caption" />


### <a name="caption"></a>캡션

항목에 사용할 캡션

 <a name="keyboard" />


### <a name="keyboard"></a>키보드

데이터 항목에 사용할 키보드 형식입니다. 가능한 값은 다음과 같습니다.

-  `ascii`
-  `decimal`
-  `default`
-  `email`
-  `name`
-  `numbers`
-  `numbers-and-punctuation`
-  `twitter`
-  `url`


 <a name="placeholder" />


### <a name="placeholder"></a>자리 표시자(placeholder)

항목에 빈 값 때 표시 되는 힌트 텍스트입니다.

 <a name="return-key" />


### <a name="return-key"></a>return-key

Return 키에 사용 되는 레이블. 가능한 값은 다음과 같습니다.

-  `default`
-  `done`
-  `emergencycall`
-  `go`
-  `google`
-  `join`
-  `next`
-  `route`
-  `search`
-  `send`
-  `yahoo`


 <a name="value" />


### <a name="value"></a>값

항목에 대 한 초기 값

 <a name="Radio_Elements" />


## <a name="radio-elements"></a>라디오 요소

라디오 요소 형식이 `"radio"`합니다. 선택 된 항목으로 선택 됩니다는 `radioselected` 속성을 포함 하는 루트 요소입니다.
또한 값에 대해 설정 된 경우는 `"group"` 속성인이 라디오 단추 그룹에 속합니다.

 <a name="Date_and_Time_Elements" />


## <a name="date-and-time-elements"></a>날짜 및 시간 요소

요소 형식은 `"datetime"`, `"date"` 및 `"time"` 렌더링 시간을 사용 하 여 날짜, 날짜 또는 시간 하는 데 사용 됩니다. 이러한 요소는 캡션 및 값 매개 변수로 합니다. .NET DateTime.Parse 함수에서 지 원하는 모든 형식의 값을 작성할 수 있습니다. 예제:

```csharp
"header": "Dates and Times",
"elements": [
        {
                "type": "datetime",
                "caption": "Date and Time",
                "value": "Sat, 01 Nov 2008 19:35:00 GMT"
        }, {
                "type": "date",
                "caption": "Date",
                "value": "10/10"
        }, {
                "type": "time",
                "caption": "Time",
                "value": "11:23"
                }                       
]
```

 <a name="Html/Web_Element" />


## <a name="htmlweb-element"></a>Html/웹 요소

만들 수 있습니다 셀 탭 할 때 지정된 된 URL의 콘텐츠를 렌더링 하는 UIWebView를 포함 하는 사용 하 여 로컬 또는 원격는 `"html"` 형식입니다. 이 요소에 대 한 두 개의 속성은 `"caption"` 고 `"url"`:

```csharp
{
        "type": "html",
        "caption": "Miguel's blog",
        "url": "https://tirania.org/blog" 
}
```
