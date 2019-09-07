---
title: 대체 리소스
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: 3f1e2ef06fb439f4b3ef290b1a7f80856b126a8d
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755194"
---
# <a name="alternate-resources"></a>대체 리소스

대체 리소스는 특정 장치 또는 런타임 구성 (예: 현재 언어, 특정 화면 크기 또는 픽셀 밀도)을 대상으로 하는 리소스입니다. Android가 기본 리소스 보다 특정 장치나 구성에 대해 보다 구체적인 리소스를 일치 시킬 수 있는 경우 해당 리소스가 대신 사용 됩니다. 현재 구성과 일치 하는 대체 리소스를 찾지 못하면 기본 리소스가 로드 됩니다. Android에서 응용 프로그램에 사용 되는 리소스를 결정 하는 방법에 대 한 자세한 내용은 리소스 위치 섹션에서 설명 합니다.

대체 리소스는 리소스 종류에 따라 리소스 폴더 내의 하위 디렉터리로 구성 됩니다 (기본 리소스와 마찬가지로). 대체 리소스 하위 디렉터리의 이름은 다음과 같은 형식입니다. _ResourceType_-_Qualifier_

*한정자* 는 특정 장치 구성을 식별 하는 이름입니다.
이름에 한정자가 둘 이상 있을 수 있으며 각 한정자는 대시로 구분 됩니다. 예를 들어 아래 스크린샷은 로캘, 화면 밀도, 화면 크기 및 방향과 같은 다양 한 구성에 대 한 대체 리소스를 포함 하는 간단한 프로젝트를 보여 줍니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![대체 리소스](alternate-resources-images/alternate-resources-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![대체 리소스](alternate-resources-images/alternate-resources-xs.png)

-----

리소스 형식에 한정자를 추가 하는 경우 다음 규칙이 적용 됩니다.

1. 한정자가 둘 이상 있을 수 있으며 각 한정자는 대시로 구분 됩니다.

2. 한정자는 한 번만 지정할 수도 있습니다.

3. 한정자는 아래 표에 표시 된 순서 대로 표시 되어야 합니다.

참조할 수 있는 한정자는 아래에 나열 되어 있습니다.

- **MCC 및 MNC** MCC ( [mobile country code](https://en.wikipedia.org/wiki/List_of_mobile_country_codes) ) 및 필요에 따라 mnc ( [모바일 네트워크 코드](https://en.wikipedia.org/wiki/Mobile_Network_Code) )입니다. &ndash; SIM 카드는 MCC를 제공 하며, 장치가 연결 된 네트워크는 MNC를 제공 합니다. 모바일 국가 코드를 사용 하 여 로캘을 대상으로 할 수 있지만 아래에 지정 된 언어 한정자를 사용 하는 것이 좋습니다. 예를 들어 리소스를 독일으로 대상으로 하는 경우 한정자 `mcc262`는입니다. 미국에서 T-Mobile에 대 한 리소스를 대상으로 하는 `mcc310-mnc026`경우 한정자는입니다.
  모바일 국가 코드 및 모바일 네트워크 코드의 전체 목록은을 참조 <http://mcc-mnc.com/>하세요.

- **언어** 두 문자로 된 [iso 639-1 언어 코드](https://en.wikipedia.org/wiki/ISO_639-1) 와 선택적으로 두 문자로 된 [iso-3166-알파-2 지역 코드가](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)옵니다. &ndash; 
  두 한정자를 모두 제공 하면로 `-r`구분 됩니다. 예를 들어 프랑스어를 대상으로 하는 로캘을 대상으로 하려면 `fr` 의 한정자가 사용 됩니다. 프랑스어-캐나다 로캘을 대상으로 하는 `fr-rCA` 경우이 사용 됩니다. 언어 코드 및 지역 코드의 전체 목록은 언어 이름 및 [국가 이름 및 코드 요소](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm) [이름 표시에 대 한 코드](http://www.loc.gov/standards/iso639-2/php/English_list.php) 를 참조 하세요.

- **가장 작은 너비** &ndash; 응용 프로그램이 실행 되는 가장 작은 화면 너비를 지정 합니다. [다양 한 화면에 대 한 리소스 만들기](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)에 자세히 설명 되어 있습니다. 
  API 수준 13 (Android 3.2) 이상에서 사용할 수 있습니다. 예를 들어 한정자 `sw320dp` 는 높이와 너비가 320dp 이상인 장치를 대상으로 하는 데 사용 됩니다.

- **사용 가능한 너비** W n dp 형식의 화면 최소 너비입니다. 여기서 *N* 은 밀도 독립 픽셀의 너비입니다. &ndash;
  사용자가 장치를 회전할 때이 값이 변경 될 수 있습니다. [다양 한 화면에 대 한 리소스 만들기](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)에 자세히 설명 되어 있습니다. 
  API 수준 13 (Android 3.2) 이상에서 사용할 수 있습니다. 예: 한정자 w720dp는 너비가 최소 720dp 인 장치를 대상으로 하는 데 사용 됩니다.

- **사용 가능한 높이** H n dp 형식의 화면 최소 높이입니다. 여기서 *N* 은 dp의 높이입니다. &ndash; 사용자가 장치를 회전할 때이 값이 변경 될 수 있습니다. [다양 한 화면에 대 한 리소스 만들기](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)에 자세히 설명 되어 있습니다. 
  API 수준 13 (Android 3.2) 이상에서 사용할 수 있습니다. 예를 들어, 한정자 h720dp는 최소 720dp의 장치를 대상으로 하는 데 사용 됩니다.

- **화면 크기** &ndash; 이 한정자는 이러한 리소스가 있는 화면 크기를 일반화 한 것입니다. [다양 한 화면에 대 한 리소스를 만드는 방법에 대해](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)자세히 설명 합니다. 
  가능한 값은 `small`, `normal`, `large` 및 `xlarge`입니다. API 수준 9에서 추가 됨 (Android 2.3/Android 2.3.1/Android 2.3.2)

- **화면 가로 세로** &ndash; 이는 화면 방향이 아니라 가로 세로 비율을 기반으로 합니다. 긴 화면이 더 넓어집니다. API 수준 4 (Android 1.6)에서 추가 되었습니다. 가능한 값은 long 및 notlong입니다.

- **화면 방향** &ndash; 세로 또는 가로 화면 방향입니다. 이는 응용 프로그램의 수명 동안 변경 될 수 있습니다.
  가능한 값은 `port` 및 `land`입니다.

- **도킹 모드** &ndash; 자동차 도크 나 책상 도크의 장치 API 수준 8 (Android 2.2. x)에서 추가 되었습니다. 가능한 값은 `car` 및 `desk`입니다.

- **야간 모드** &ndash; 응용 프로그램을 야간 또는 하루에 실행할지 여부입니다. 이는 응용 프로그램의 수명 동안 변경 될 수 있으며, 개발자에 게 밤에 더 어두운 버전의 인터페이스를 사용할 수 있는 기회를 제공 하기 위한 것입니다. API 수준 8 (Android 2.2. x)에서 추가 되었습니다. 가능한 값은 `night` 및 `notnight`입니다.

- **화면 픽셀 밀도 (dpi)** &ndash; 실제 화면에서 지정 된 영역의 픽셀 수입니다. 일반적으로 dpi (인치당 도트 수)로 표시 됩니다. 가능한 값은

  - `ldpi`&ndash; 낮은 밀도 화면.

  - `mdpi`&ndash; 보통 밀도 화면

  - `hdpi`&ndash; 고밀도 화면

  - `xhdpi`&ndash; 추가 고밀도 화면

  - `nodpi`&ndash; 크기를 조정 하지 않을 리소스

  - `tvdpi`&ndash; Mdpi와 hdpi 사이의 화면에 대 한 API 수준 13 (Android 3.2)에서 도입 되었습니다.

- **터치 스크린 형식** &ndash; 장치에 포함 될 수 있는 터치 스크린의 유형을 지정 합니다. 가능한 값은 `notouch` (터치 스크린 없음), `stylus` (스타일러스에 적합 한 resistive 터치 스크린) 및 `finger` (터치 스크린)입니다.

- **키보드 가용성** &ndash; 사용할 수 있는 키보드 종류를 지정 합니다. 이는 사용자가 하드웨어 키보드를 열 때 &ndash; 예를 들어 응용 프로그램의 수명 동안 변경 될 수 있습니다. 가능한 값은

  - `keysexposed`&ndash; 장치에 사용할 수 있는 키보드가 있습니다. 사용 하도록 설정 된 소프트웨어 키보드가 없으면 하드웨어 키보드가 열려 있는 경우에만 사용 됩니다.

  - `keyshidden`&ndash; 장치에 하드웨어 키보드가 있지만 숨겨진 상태 이며 소프트웨어 키보드를 사용할 수 없습니다.

  - `keyssoft`&ndash; 장치에서 소프트웨어 키보드를 사용할 수 있습니다.

- **기본 텍스트 입력 방법** &ndash; 입력에 사용할 수 있는 하드웨어 키 종류를 지정 하는 데 사용 합니다. 가능한 값은

  - `nokeys`&ndash; 입력에 대 한 하드웨어 키가 없습니다.

  - `qwerty`&ndash; 사용할 수 있는 qwerty 키보드가 있습니다.

  - `12key`&ndash; 12 개의 키 하드웨어 키보드가 있습니다.

- **탐색 키 가용성** &ndash; 5 방향 또는 d-패드 (방향 패드) 탐색을 사용할 수 있는 경우. 이는 응용 프로그램의 수명 동안 변경 될 수 있습니다. 가능한 값은

  - `navexposed`&ndash; 사용자가 탐색 키를 사용할 수 있습니다.

  - `navhidden`&ndash; 탐색 키를 사용할 수 없습니다.

- **기본 비 터치 탐색 방법** &ndash; 장치에서 사용할 수 있는 탐색의 종류입니다. 가능한 값은

  - `nonav`&ndash; 터치 스크린만 사용할 수 있는 탐색 기능이 있습니다.

  - `dpad`&ndash; d-패드 (방향 패드)를 탐색에 사용할 수 있습니다.

  - `trackball`&ndash; 장치에 탐색을 위한 트랙볼이 있습니다.

  - `wheel`&ndash; 하나 이상의 방향성 바퀴를 사용할 수 있는 드문 시나리오

- **플랫폼 버전 (API 수준)** V n 형식의 장치에서 지원 되는 api 수준으로, 여기서 *N* 은 대상으로 하는 api 수준입니다. &ndash; 예를 들어 v11는 API 레벨 11 (Android 3.0) 장치를 대상으로 합니다.

리소스 한정자에 대 한 자세한 내용은 Android 개발자 웹 사이트에서 [리소스 제공](https://developer.android.com/guide/topics/resources/providing-resources.html) 을 참조 하세요.

## <a name="how-android-determines-what-resources-to-use"></a>Android에서 사용할 리소스를 결정 하는 방법

Android 응용 프로그램에는 많은 리소스가 포함 될 가능성이 높습니다. Android가 장치에서 실행 될 때 응용 프로그램에 대 한 리소스를 선택 하는 방법을 이해 하는 것이 중요 합니다.

Android는 다음 규칙 테스트를 반복 하 여 리소스 베이스를 결정 합니다.

- **모순 되는 한정자 제거** &ndash; 예를 들어, 장치 방향이 세로 이면 모든 가로 리소스 디렉터리가 거부 됩니다.

- **지원 되지 않는 한정자 무시** &ndash; 모든 한정자를 모든 API 수준에서 사용할 수 있는 것은 아닙니다. 리소스 디렉터리에 장치에서 지원 하지 않는 한정자가 포함 된 경우 해당 리소스 디렉터리는 무시 됩니다.

- **다음으로 높은 우선 순위 한정자를 식별 합니다** . &ndash; 위의 표를 참조 하 여 우선 순위가 가장 높은 다음 한정자 (위에서 아래로)를 선택 합니다.

- **한정자에 대 한 리소스 디렉터리 유지** &ndash; 위의 테이블에 대 한 한정자와 일치 하는 리소스 디렉터리가 있으면 위쪽에서 아래쪽으로 가장 높은 우선 순위 한정자를 선택 합니다.

이러한 규칙은 다음 순서도에도 설명 되어 있습니다.

[![리소스 순서도](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

시스템에서 밀도 별 리소스를 찾고 찾을 수 없는 경우 다른 밀도 특정 리소스를 찾고 크기를 조정 하려고 합니다. Android는 기본 리소스를 반드시 사용할 필요는 없습니다.
예를 들어 밀도가 낮은 리소스를 찾을 때 사용할 수 없는 리소스를 사용할 수 없는 경우 Android에서 기본 또는 중간 밀도 리소스에 대해 고밀도 버전의 리소스를 선택할 수 있습니다. 이는 고밀도 리소스를 0.5의 비율로 축소할 수 있으므로이를 수행 합니다 .이 경우 0.75의 요인이 필요한 중간 밀도 리소스를 축소 하는 것 보다 가시성 문제가 줄어듭니다.

예를 들어, 다음과 같이 그릴 때 리소스 디렉터리가 포함 된 응용 프로그램을 가정해 보겠습니다.

```
drawable
drawable-en
drawable-fr-rCA
drawable-en-port
drawable-en-notouch-12key
drawable-en-port-ldpi
drawable-port-ldpi
drawable-port-notouch-12key
```

이제 응용 프로그램이 다음 구성을 사용 하 여 장치에서 실행 됩니다.

- **로캘** &ndash; en-us
- **방향** &ndash; 포트
- **화면 밀도** &ndash; hdpi
- **터치 스크린 형식** &ndash; notouch
- **기본 입력 방법** &ndash; 12key

먼저 프랑스어 리소스는의 `en-GB`로캘과 충돌 하 고 다음을 사용 하 여 제거 됩니다.

```
drawable
drawable-en
drawable-en-port
drawable-en-notouch-12key
drawable-en-port-ldpi
drawable-port-ldpi
drawable-port-notouch-12key
```

다음으로, 위의 한정자 표에서 첫 번째 한정자를 선택 합니다. MCC 및 MNC. 이 한정자를 포함 하는 리소스 디렉터리가 없으므로 MCC/MNC 코드가 무시 됩니다.

다음 한정자가 선택 됩니다 (언어). 언어 코드와 일치 하는 리소스가 있습니다. 의 `en` 언어 코드와 일치 하지 않는 모든 리소스 디렉터리가 거부 되므로 이제 리소스 목록이 다음과 같이 표시 됩니다.

```
drawable-en-port
drawable-en-notouch-12key
drawable-en-port-ldpi
```

다음 한정자는 화면 방향에 대 한 것 이므로의 `port` 화면 방향과 일치 하지 않는 모든 리소스 디렉터리가 제거 됩니다.

```
drawable-en-port
drawable-en-port-ldpi
```

다음은 하나 이상의 리소스 디렉터리를 제외 `ldpi`하는 화면 밀도의 한정자입니다.

```
drawable-en-port-ldpi
```

이 프로세스의 결과로 Android는 장치에 대 한 리소스 디렉터리 `drawable-en-port-ldpi` 에서 그릴 수 있는 리소스를 사용 합니다.

> [!NOTE]
> 화면 크기 한정자는이 선택 프로세스에 한 가지 예외를 제공 합니다. Android에서 현재 장치가 제공 하는 것 보다 더 작은 화면으로 디자인 된 리소스를 선택할 수 있습니다. 예를 들어 큰 화면 장치에서는 일반적인 크기의 화면에 대해 제공 하는 리소스를 사용할 수 있습니다. 그러나 반대의 경우는 그렇지 않습니다. 동일한 긴 화면 장치는 초대형 화면에 제공 된 리소스를 사용 하지 않습니다. Android에서 지정 된 화면 크기와 일치 하는 리소스 집합을 찾을 수 없는 경우 응용 프로그램의 작동이 중단 됩니다.
