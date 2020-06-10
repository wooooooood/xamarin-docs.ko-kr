---
title: MonoTouch.Dialog Json 태그
description: 이 문서에서는 Monotouch.dialog를 사용 하 여 Xamarin.ios 사용자 인터페이스를 작성 하는 데 사용할 수 있는 JSON 구문을 설명 합니다.
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: fc6066155a4171b106e772c1fe6fe7ee3e5c67cf
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573510"
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog Json 태그

이 페이지에서는 Monotouch.dialog에서 허용 하는 Json 태그에 대해 설명 합니다. 대화 상자의 [JsonElement](xref:MonoTouch.Dialog.JsonElement)

예제로 시작 해 보세요. 다음은 JsonElement에 전달할 수 있는 전체 Json 파일입니다.

```json
{     
    "title": "Json Sample",
    "sections": [ 
        {
            "header": "Booleans",
            "footer": "Slider or image-based",
            "id": "first-section",
            "elements": [
                { 
                    "type": "boolean",
                    "caption": "Demo of a Boolean",
                    "value": true
                }, {
                    "type": "boolean",
                    "caption": "Boolean using images",
                    "value": false,
                    "on": "favorite.png",
                    "off": "~/favorited.png"
                }, {
                    "type": "root",
                    "title": "Tap for nested controller",
                    "sections": [
                        {
                            "header": "Nested view!",
                            "elements": [
                                {
                                    "type": "boolean",
                                    "caption": "Just a boolean",
                                    "id": "the-boolean",
                                    "value": false
                                }, {
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

위의 태그는 다음 UI를 생성 합니다.

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "The UI created by the given markup")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

트리의 모든 요소에는 속성이 포함 될 수 있습니다 `"id"` . 런타임에 JsonElement 인덱서를 사용 하 여 개별 섹션 또는 요소를 참조할 수 있습니다. 다음과 같이:

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement;
```

 <a name="Root_Element_Syntax"></a>

## <a name="root-element-syntax"></a>Root 요소 구문

Root 요소는 다음 값을 포함 합니다.

- `title`
- `sections`(선택 사항)

루트 요소는 중첩 된 컨트롤러를 만들기 위한 요소로 섹션 안에 나타날 수 있습니다. 이 경우 추가 속성을 `"type"` 로 설정 해야 합니다.`"root"`

 <a name="url"></a>

### <a name="url"></a>url

`"url"`속성이 설정 된 경우 사용자가이 RootElement를 누르면 코드가 지정 된 url에서 파일을 요청 하 고 콘텐츠를 표시 하는 새 정보를 만듭니다. 이를 사용 하 여 사용자가 탭 하는 항목에 따라 서버에서 사용자 인터페이스를 확장할 수 있습니다.

 <a name="group"></a>

### <a name="group"></a>group

설정 하는 경우 루트 요소의 groupname을 설정 합니다. 그룹 이름은 요소의 중첩 된 요소 중 하나에서 루트 요소의 값으로 표시 되는 요약을 선택 하는 데 사용 됩니다. 이 값은 확인란 또는 라디오 단추의 값입니다.

 <a name="radioselected"></a>

### <a name="radioselected"></a>radioselected

중첩 된 요소에서 선택 된 라디오 항목을 식별 합니다.

 <a name="title"></a>

### <a name="title"></a>title

있는 경우 RootElement에 사용 되는 제목입니다.

 <a name="type"></a>

### <a name="type"></a>type

`"root"`섹션에 표시 될 때 (컨트롤러를 중첩 하는 데 사용 됨)로 설정 해야 합니다.

 <a name="sections"></a>

### <a name="sections"></a>섹션

개별 섹션이 포함 된 Json 배열입니다.

 <a name="Section_Syntax"></a>

## <a name="section-syntax"></a>섹션 구문

섹션에는 다음이 포함 됩니다.

- `header`(선택 사항)
- `footer`(선택 사항)
- `elements` 배열

 <a name="header"></a>

### <a name="header"></a>header

표시 되는 경우 머리글 텍스트가 섹션의 캡션으로 표시 됩니다.

 <a name="footer"></a>

### <a name="footer"></a>바닥글

있는 경우 바닥글은 섹션의 아래쪽에 표시 됩니다.

 <a name="elements"></a>

### <a name="elements"></a>요소

이는 요소의 배열입니다. 각 요소에는 `"type"` 만들 요소의 종류를 식별 하는 데 사용 되는 키 인 키가 하나 이상 포함 되어야 합니다.
일부 요소는 및와 같은 몇 가지 공통 속성 `"caption"` 을 공유 `"value"` 합니다. 다음은 지원 되는 요소 목록입니다.

- `string`요소 (스타일 지정을 사용 하거나 사용 하지 않음)
- `entry`줄 (일반 또는 암호)
- `boolean`값 (스위치 또는 이미지 사용)

사용자가 셀 또는 액세서리에서 탭 할 때 호출할 메서드를 제공 하 여 문자열 요소를 단추로 사용할 수 있습니다.

 <a name="Rendering_Elements"></a>

## <a name="rendering-elements"></a>렌더링 요소

렌더링 요소는 c # Stringelement 및 StyledStringElement를 기반으로 하며 다양 한 방법으로 정보를 렌더링할 수 있으며 다양 한 방법으로 렌더링할 수 있습니다. 가장 간단한 요소를 다음과 같이 만들 수 있습니다.

```json
{
    "type": "string",
    "caption": "Json Serializer"
}
```

그러면 모든 기본값을 포함 하는 간단한 문자열이 표시 됩니다 (글꼴, 배경, 텍스트 색 및 장식). 이러한 요소에 작업을 연결 하 고 속성 또는 속성을 설정 하 여 단추 처럼 동작 하도록 할 수 있습니다 `"ontap"` `"onaccessorytap"` .

```json
{
    "type": "string",
    "caption": "View Photos",
    "ontap": "Acme.PhotoLibrary.ShowPhotos"
}
```

위의은 "PhotoLibrary" 클래스에서 "ShowPhotos" 메서드를 호출 합니다. 는 `"onaccessorytap"` 비슷하지만 사용자가 셀을 누르는 대신 액세서리에서 탭 하는 경우에만 호출 됩니다. 이를 사용 하도록 설정 하려면 액세서리도 설정 해야 합니다.

```json
{
    "type": "string",
    "caption": "View Photos",
    "ontap": "Acme.PhotoLibrary.ShowPhotos",
    "accessory": "detail-disclosure",
    "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

렌더링 요소는 한 번에 두 문자열을 표시할 수 있습니다. 하나는 캡션과 다른 하나는 값입니다. 이러한 문자열을 렌더링 하는 방법은 스타일에 따라 달라 지 며, 속성을 사용 하 여 설정할 수 있습니다 `"style"` . 기본적으로 왼쪽에는 캡션이 표시 되 고 오른쪽에는 값이 표시 됩니다. 자세한 내용은 스타일에 대 한 섹션을 참조 하세요. 색은 ' # ' 기호와 빨간색, 녹색, 파랑 및 가능한 알파 값의 값을 나타내는 16 진수 번호를 사용 하 여 인코딩됩니다. 콘텐츠는 RGB 또는 RGBA 값을 나타내는 짧은 형식 (3 또는 4 자리 16 진수)으로 인코딩할 수 있습니다. RGB 또는 RGBA 값을 나타내는 긴 형식 (6 자리 또는 8 자리)입니다. 짧은 버전은 동일한 16 진수를 두 번 쓰는 축약형입니다. 따라서 "#1bc" 상수는 red = 0x11, green = 0xbb 및 blue = 0xbb로 intepreted 됩니다. 알파 값이 없으면 색은 불투명 합니다. 몇 가지 예는 다음과 같습니다.

```json
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory"></a>

### <a name="accessory"></a>액세서리

렌더링 요소에 표시할 액세서리 종류를 결정 합니다. 가능한 값은 다음과 같습니다.

- `checkmark`
- `detail-disclosure`
- `disclosure-indicator`

값이 없는 경우에는 액세서리는 표시 되지 않습니다.

 <a name="background"></a>

### <a name="background"></a>background

배경 속성은 셀의 배경색을 설정 합니다. 값은 이미지에 대 한 URL (이 경우에는 비동기 이미지 다운로더를 호출 하 고, 이미지를 다운로드 한 후에는 배경을 업데이트 함) 이거나 색 구문을 사용 하 여 지정 된 색 일 수 있습니다.

 <a name="caption"></a>

### <a name="caption"></a>caption

렌더링 요소에 표시할 주 문자열입니다. 및 속성을 설정 하 여 글꼴 및 색을 사용자 지정할 수 있습니다 `"textcolor"` `"font"` . 렌더링 스타일은 속성에 의해 결정 됩니다 `"style"` .

 <a name="color_and_detailcolor"></a>

### <a name="color-and-detailcolor"></a>color 및 detailcolor

주 텍스트 또는 세부 텍스트에 사용할 색입니다.

 <a name="detailfont_and_font"></a>

### <a name="detailfont-and-font"></a>detailfont 및 글꼴

캡션 또는 정보 텍스트에 사용할 글꼴입니다. 글꼴 사양의 형식은 글꼴 이름과 대시 및 포인트 크기를 선택적으로 지정 합니다.
유효한 글꼴 사양은 다음과 같습니다.

- Helvetica
- "Helvetica-14"

 <a name="linebreak"></a>

### <a name="linebreak"></a>linebreak

줄 분할 방법을 결정 합니다. 가능한 값은 다음과 같습니다.

- `character-wrap`
- `clip`
- `head-truncation`
- `middle-truncation`
- `tail-truncation`
- `word-wrap`

`character-wrap`및 `word-wrap` 는 모두 `"lines"` 0으로 설정 된 속성을 사용 하 여 렌더링 요소를 여러 줄 요소로 변환할 수 있습니다.

 <a name="ontap_and_onaccessorytap"></a>

### <a name="ontap-and-onaccessorytap"></a>ontap 및 onaccessorytap

이러한 속성은 개체를 매개 변수로 사용 하는 응용 프로그램의 정적 메서드 이름을 가리켜야 합니다. JsonDialog 또는 JsonDialog 메서드를 사용 하 여 계층을 만들 때 선택적 개체 값을 전달할 수 있습니다. 그런 다음이 개체 값이 메서드에 전달 됩니다. 이를 사용 하 여 정적 메서드에 일부 컨텍스트를 전달할 수 있습니다. 예를 들면 다음과 같습니다.

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

 <a name="lines"></a>

### <a name="lines"></a>lines

이 값을 0으로 설정 하면 포함 된 문자열의 내용에 따라 요소가 자동으로 크기가 지정 됩니다. 이 작업을 수행 하려면 `"linebreak"` 속성을 또는로 설정 해야 합니다 `"character-wrap"` `"word-wrap"` .

 <a name="style"></a>

### <a name="style"></a>스타일

스타일은 콘텐츠를 렌더링 하는 데 사용 되는 셀 스타일의 종류를 결정 하 고 UITableViewCellStyle 열거형 값에 해당 합니다.
가능한 값은 다음과 같습니다.

- `"default"`
- `"value1"`
- `"value2"`
- `"subtitle"`: 부제목이 있는 텍스트입니다.

 <a name="subtitle"></a>

### <a name="subtitle"></a>제목

부제목에 사용할 값입니다. 스타일을로 설정 하 `"subtitle"` 고 속성을 문자열로 설정 하는 바로 가기입니다 `"value"` .
단일 항목을 사용 합니다.

 <a name="textcolor"></a>

### <a name="textcolor"></a>textcolor

텍스트에 사용할 색입니다.

 <a name="value"></a>

### <a name="value"></a>값

렌더링 요소에 표시할 보조 값입니다. 이의 레이아웃은 설정의 영향을 받습니다 `"style"` . 글꼴과 색은 및를 설정 하 여 사용자 지정할 수 있습니다 `"detailfont"` `"detailcolor"` .

 <a name="Boolean_Elements"></a>

## <a name="boolean-elements"></a>부울 요소

부울 요소는 형식을로 설정 해야 하 `"bool"` 고,를 표시 하기 위해를 포함할 수 있으며, `"caption"` `"value"` 가 true 또는 false로 설정 됩니다. `"on"`및 `"off"` 속성을 설정 하는 경우 이미지로 간주 됩니다. 이미지는 응용 프로그램의 현재 작업 디렉터리를 기준으로 확인 됩니다. 번들 관련 파일을 참조 하려는 경우를 바로 가기로 사용 하 여 `"~"` 응용 프로그램 번들 디렉터리를 나타낼 수 있습니다. 예를 들어 `"~/favorite.png"` , 번들 파일에 포함 된 즐겨찾기나 .png를 사용할 수 있습니다. 예를 들면 다음과 같습니다.

```json
{ 
    "type": "boolean",
    "caption": "Demo of a Boolean",
    "value": true
},

{
    "type": "boolean",
    "caption": "Boolean using images",
    "value": false,
    "on": "favorite.png",
    "off": "~/favorited.png"
}
```

 <a name="type"></a>

### <a name="type"></a>type

형식은 또는로 설정할 수 있습니다 `"boolean"` `"checkbox"` . 부울로 설정 하는 경우 UISlider 또는 이미지를 사용 `"on"` 합니다 (및 `"off"` 가 모두 설정 된 경우). Checkbox로 설정 되 면 확인란을 사용 합니다. `"group"`속성을 사용 하 여 부울 요소에 특정 그룹에 속하는 것으로 태그를 지정할 수 있습니다. 이 방법은 포함 하는 루트에도 `"group"` 동일한 그룹에 속하는 모든 부울 (또는 확인란) 수를 사용 하 여 결과를 요약 하는 루트가 포함 된 속성을 포함 하는 경우에 유용 합니다.

 <a name="Entry_Elements"></a>

## <a name="entry-elements"></a>Entry 요소

입력 요소를 사용 하 여 사용자가 데이터를 입력할 수 있습니다. Entry 요소의 형식은 `"entry"` 또는 `"password"` 입니다. `"caption"`속성은 오른쪽에 표시할 텍스트로 설정 되 고는 `"value"` 항목을 설정 하기 위한 초기 값으로 설정 됩니다. 는 `"placeholder"` 사용자에 게 빈 항목 (회색으로 표시 됨)에 대 한 힌트를 표시 하는 데 사용 됩니다. 몇 가지 예제는 다음과 같습니다.

```json
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

 <a name="autocorrect"></a>

### <a name="autocorrect"></a>고침은

항목에 사용할 자동 수정 스타일을 결정 합니다. 가능한 값은 true 또는 false (또는 문자열 `"yes"` 및 `"no"` )입니다.

 <a name="capitalization"></a>

### <a name="capitalization"></a>대/소문자

항목에 사용할 대문자 표시 스타일입니다. 가능한 값은 다음과 같습니다.

- `all`
- `none`
- `sentences`
- `words`

 <a name="caption"></a>

### <a name="caption"></a>caption

항목에 사용할 캡션입니다.

 <a name="keyboard"></a>

### <a name="keyboard"></a>키보드

데이터 입력에 사용할 키보드 유형입니다. 가능한 값은 다음과 같습니다.

- `ascii`
- `decimal`
- `default`
- `email`
- `name`
- `numbers`
- `numbers-and-punctuation`
- `twitter`
- `url`

 <a name="placeholder"></a>

### <a name="placeholder"></a>자리 표시자(placeholder)

항목에 빈 값이 있을 때 표시 되는 힌트 텍스트입니다.

 <a name="return-key"></a>

### <a name="return-key"></a>반환 키

반환 키에 사용 되는 레이블입니다. 가능한 값은 다음과 같습니다.

- `default`
- `done`
- `emergencycall`
- `go`
- `google`
- `join`
- `next`
- `route`
- `search`
- `send`
- `yahoo`

 <a name="value"></a>

### <a name="value"></a>값

항목의 초기 값입니다.

 <a name="Radio_Elements"></a>

## <a name="radio-elements"></a>Radio 요소

Radio 요소는 형식 `"radio"` 입니다. 선택한 항목은 `radioselected` 포함 하는 루트 요소의 속성에 의해 선택 됩니다.
또한 속성에 값이 설정 된 경우 `"group"` 이 라디오 단추는 해당 그룹에 속합니다.

 <a name="Date_and_Time_Elements"></a>

## <a name="date-and-time-elements"></a>날짜 및 시간 요소

요소 형식 `"datetime"` 및는 `"date"` `"time"` 시간, 날짜 또는 시간을 사용 하 여 날짜를 렌더링 하는 데 사용 됩니다. 이러한 요소는 캡션과 값을 매개 변수로 사용 합니다. 값은 .NET DateTime. Parse 함수에서 지 원하는 모든 형식으로 작성할 수 있습니다. 예제:

```json
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

 <a name="Html/Web_Element"></a>

## <a name="htmlweb-element"></a>Html/Web 요소

형식을 사용 하 여 지정 된 URL (로컬 또는 원격)의 콘텐츠를 렌더링 하는 UIWebView 보기를 탭 할 때 포함 하는 셀을 만들 수 있습니다 `"html"` . 이 요소에 대 한 두 가지 속성은 및 뿐입니다 `"caption"` `"url"` .

```json
{
    "type": "html",
    "caption": "Miguel's blog",
    "url": "https://tirania.org/blog" 
}
```
