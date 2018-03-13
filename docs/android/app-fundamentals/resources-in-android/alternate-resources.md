---
title: "대체 리소스"
ms.topic: article
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 7ebbf2a9215c8472ae2f286728cb2f819e8331cb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="alternate-resources"></a>대체 리소스

다른 리소스는 특정 장치 또는 현재 언어, 특정 화면 크기 또는 픽셀 밀도 같은 런타임 구성을 대상으로 하는 리소스입니다. Android에는 리소스는 특정 장치 또는 구성에 대 한 보다 구체적인 기본 리소스를 일치 시킬 수, 한 해당 리소스 대신 사용 됩니다. 현재 구성과 일치 하는 대체 리소스를 찾을 수 없으면 기본 리소스 로드 됩니다. 리소스 위치 섹션에서 자세히 아래에서 응용 프로그램에서 사용할 리소스를 설명 합니다 Android 결정 하는 방법

다른 리소스는 기본 리소스와 마찬가지로 리소스 종류에 따라 리소스 폴더에 하위 디렉터리를으로 구성 됩니다. 대체 리소스 하위 디렉터리의 이름은 형태로 표현: _ResourceType_-_한정자_

*한정자* 특정 장치 구성을 식별 하는 이름입니다.
각각의 대시로 구분 된 이름에서 한정자를 여러 개 있을 수 있습니다. 예를 들어, 아래 스크린샷에서 간단한 포함 된 프로젝트를 로캘, 화면 밀도, 화면 크기 및 방향과 같은 다양 한 구성에 대 한 대체 리소스를 보여 줍니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![대체 리소스](alternate-resources-images/alternate-resources-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![대체 리소스](alternate-resources-images/alternate-resources-xs.png)
 
-----
 

리소스 종류에 한정자를 추가 하는 경우 다음 규칙에 적용 됩니다.

1. 둘 이상의 한정자, 대시로 구분 된 각 한정자와 함께 있을 수 있습니다.

2. 한 번만 지정 미정 한정자입니다.

3. 한정자는 아래 테이블에 나타나는 순서에 있어야 합니다.

참조에 대 한 가능한 한정자는 다음과 같습니다.

- **MCC 및 MNC** &ndash; 는 [모바일 국가 코드](http://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MCC) 및 필요에 따라는 [모바일 네트워크 코드](http://en.wikipedia.org/wiki/Mobile_Network_Code) (MNC). SIM 카드는 MNC를 제공 하는 장치에 연결 된 네트워크는 MCC를 제공 합니다. 모바일 국가 코드를 사용 하 여 대상 로캘로 수 있지만 아래에 지정 된 언어 한정자를 사용 하는 권장 방법이입니다. 예를 들어, 대상 리소스에 독일에 한정자 것 `mcc262`합니다. 미국에서는 T Mobile에 대 한 대상 리소스에 한정자가 `mcc310-mnc026`합니다.
  모바일 국가 코드와 모바일 네트워크 코드의 전체 목록은 참조 <http://mcclist.com/>합니다.

- **언어** &ndash; 두 문자로 [ISO 639-1 언어 코드](http://en.wikipedia.org/wiki/ISO_639-1) 및 필요에 따라 두 문자 뒤 [ISO 3166-알파 2 지역 코드](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)합니다. 
  두 한정자를 사용할 경우로 구분 됩니다는 `-r`합니다. 예를 들어 대상 로캘을 프랑스어 다음의 한정자 `fr` 사용 됩니다. 캐나다 로캘에서 대상으로 `fr-rCA` 사용 됩니다. 언어 코드 및 지역 코드의 전체 목록은 참조 하십시오. [표현의 이름은의 언어에 대 한 코드](http://www.loc.gov/standards/iso639-2/php/English_list.php) 및 [국가 이름 및 코드 요소](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm)합니다.

- **가장 작은 두께** &ndash; 응용 프로그램에서 실행 하려는 가장 작은 화면 너비를 지정 합니다. 좀 더 자세히 다룰 [다양 한 화면에 대 한 리소스를 만드는](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)합니다. 
  이상 (Android 3.2) 13 API 수준에서 사용할 수 있습니다. 예를 들어 한정자 `sw320dp` 해당 높이 너비는 적어도 장치를 대상으로 하는 데 320dp 합니다.

- **사용 가능한 너비** &ndash; 형식 w에서 화면의 최소 너비*N*dp, 여기서 *N* 독립적 픽셀 밀도에 너비입니다.
  사용자가 장치를 회전할이 값이 변경 될 수 있습니다. 좀 더 자세히 다룰 [다양 한 화면에 대 한 리소스를 만드는](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)합니다. 
  API 수준 (Android 3.2) 13 이상에서 사용할 수 있습니다. 예: 한정자 w720dp 최소 720dp 너비가 장치를 대상으로 하는 데 사용 됩니다.

- **사용 가능한 높이** &ndash; 형식 h에서 화면의 최소 높이*N*dp, 여기서 *N* dp의 높이 같습니다. 사용자가 장치를 회전할이 값이 변경 될 수 있습니다. 좀 더 자세히 다룰 [다양 한 화면에 대 한 리소스를 만드는](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)합니다. 
  API 수준 (Android 3.2) 13 이상에서 사용할 수 있습니다. 예를 들어 한정자 h720dp은 최소 720dp의 높이가 장치를 대상으로 하는 데 사용 됩니다.

- **화면 크기** &ndash; 이 한정자는이 리소스는에 대 한 화면 크기의 일반화 합니다. 더 자세하게에 대해서는 [다양 한 화면에 대 한 리소스를 만드는](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)합니다. 
  가능한 값은 `small`, `normal`, `large` 및 `xlarge`입니다. API 수준 9 (Android 2.3/Android 2.3.1/Android 2.3.2)에 추가

- **화면 가로 세로** &ndash; 이 설정은 화면 방향 가로 세로 비율에 기반 합니다. 긴 화면이 큽니다. API 수준 (Android 1.6) 4에에서 추가 합니다. 가능한 값은 long 및 notlong입니다.

- **화면 방향을** &ndash; 세로 또는 가로 화면 방향입니다. 이 응용 프로그램의 수명 동안 변경할 수 있습니다.
  가능한 값은 `port`와 `land`입니다.

- **도킹 모드** &ndash; 자동차의 장치 도킹 하거나 도킹 책상에 대 한 합니다. API 수준 8 (Android 2.2. x)에 추가 합니다. 가능한 값은 `car`와 `desk`입니다.

- **야간 모드** &ndash; 여부 응용 프로그램이 실행 중인 밤 또는에 있습니다. 개발자가 밤 인터페이스의 짙을 수록 버전을 사용할 수 있도록 것 이며 응용 프로그램의 수명 동안 변경 될 수 있습니다. API 수준 8 (Android 2.2. x)에 추가 합니다. 가능한 값은 `night`와 `notnight`입니다.

- **화면 픽셀 밀도 (dpi)** &ndash; 실제 화면에 지정 된 영역에는 픽셀 수입니다. 일반적으로 (dpi)로 표현 됩니다. 가능한 값은 다음과 같습니다.

    - `ldpi` &ndash; 낮은 밀도 화면입니다.

    - `mdpi` &ndash; 중간 밀도 화면

    - `hdpi` &ndash; 밀도가 높은 화면

    - `xhdpi` &ndash; 특 고밀도 화면

    - `nodpi` &ndash; 크기가 조정 되지 않는 리소스

    - `tvdpi` &ndash; API 수준 (Android 3.2) 13 화면에 대 한 mdpi hdpi 사이에서 도입 되었습니다.

- **터치 스크린이 형식** &ndash; 터치 스크린을 장치에 있을 수의 유형을 지정 합니다. 가능한 값은 `notouch` (터치 스크린 없음), `stylus` (저항 식 터치 스크린 스타일러스에 적합 한), 및 `finger` (터치 스크린이).

- **가용성 키보드** &ndash; 수 있는 키보드의 종류를 지정 합니다. 응용 프로그램의 수명 동안 변경 될 수 있습니다이 &ndash; 예를 들어 사용자가 열 때 하드웨어 키보드 합니다. 가능한 값은 다음과 같습니다.

    - `keysexposed` &ndash; 장치에는 바로 사용할 수 있습니다. 사용할 수 없는 소프트웨어 키보드 이면 다음이 사용 됩니다 하드웨어 키보드를 열 때.

    - `keyshidden` &ndash; 장치 하드웨어 키보드는 숨겨져 있고 없는 소프트웨어 키보드를 사용할 수 있습니다. 합니다.

    - `keyssoft` &ndash; 장치에 소프트웨어 키보드를 사용 하도록 설정 합니다.

- **기본 텍스트 입력 방법** &ndash; 사용 하 여 어떤 종류의 하드웨어 키 입력에 대해 사용할 수를 지정 합니다. 가능한 값은 다음과 같습니다.

    - `nokeys` &ndash; 입력에 대 한 하드웨어 키가 있습니다.

    - `qwerty` &ndash; Qwerty 바로 제공 됩니다.

    - `12key` &ndash; 12 키 하드웨어 키보드는


- **탐색 키 가용성** &ndash; (방향 패드) 탐색 패드 또는 5 방식으로 제공 된 경우에 대 한 합니다. 이 응용 프로그램의 수명 동안 변경할 수 있습니다. 가능한 값은 다음과 같습니다.

    - `navexposed` &ndash; 탐색 키가 사용자에 게 사용할 수

    - `navhidden` &ndash; 탐색 키는 사용할 수 없습니다.

-  **기본 비 터치 탐색 방법** &ndash; 장치에서 사용할 수 있는 탐색의 종류입니다. 가능한 값은 다음과 같습니다.

    - `nonav` &ndash; 사용 가능한 유일한 탐색 시설은 터치 스크린

    - `dpad` &ndash; 방향 패드 (방향 패드)는 탐색에 사용할 수

    - `trackball` &ndash; 장치에 탐색을 위한 트랙볼

    - `wheel` &ndash; 하나 또는 위치 자세한 방향 바퀴 사용할 수 있는 일반적인 시나리오

-  **플랫폼 버전 (API 수준)** &ndash; v 형식으로 장치에서 지 원하는 API 수준*N*여기서 *N* 대상이 되는 API 수준입니다. V11 API 수준 11 (Android 3.0)를 대상 하는 예를 들어 장치입니다.


리소스에 대 한 자세한 내용은 한정자 참조 [리소스를 제공 하](http://developer.android.com/guide/topics/resources/providing-resources.html) Android 개발자 웹 사이트입니다.


## <a name="how-android-determines-what-resources-to-use"></a>Android에서 사용할 리소스를 결정 하는 방법

것은 가능 하 고 Android 응용 프로그램 리소스가 많이 들어 있습니다. 방법을 통해 Android 장치에서 실행 될 경우 응용 프로그램에 대 한 리소스를 선택 합니다 이해 하는 것이 유용 합니다.

Android 규칙의 다음 테스트를 반복 하 여 기본 리소스를 결정 합니다.

- **모순 한정자를 제거** &ndash; 예를 들어 장치 방향을 세로 이면 다음 모든 가로 리소스 디렉터리 거부 됩니다.

- **지원 되지 않습니다 한정자를 무시** &ndash; 모든 한정자는 모든 API 수준에 사용할 수 있습니다. 리소스 디렉터리 장치에서 지원 되지 않는 한정자 들어 있는 경우 해당 리소스 디렉터리 무시 됩니다.

- **다음으로 가장 높은 우선 순위 한정자 식별** &ndash; (위에서 아래로) 다음 가장 높은 우선 순위 한정자의 선택을 위 테이블을 참조 합니다.

- **한정자에 대 한 모든 리소스 디렉터리 유지** &ndash; 위의 테이블에 한정자와 일치 하는 모든 리소스 디렉터리에 있는 경우 (위에서 아래로) 다음 가장 높은 우선 순위 한정자를 선택 합니다.

이러한 규칙은 다음 순서도에 확인할 수 있습니다.

[![리소스 순서도](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

시스템에서 밀도 별 리소스를 찾고을 찾을 수 없는 때 다른 밀도 특정 리소스를 찾아 확장 하거나 시도 합니다. Android 기본 리소스를 반드시 사용 하지 않을 수 있습니다.
예를 들어 저밀도 리소스와이 사용할 수 없는 Android 고밀도 리소스의 버전을 기본 또는 중간 밀도 리소스에 대해 선택할 수 있습니다. 0.5, 0.75 배 해야 하는 중간 밀도 리소스 축소 보다 더 적은 가시성 문제 그러면 계수로 고밀도 리소스 축소 될 수 있으므로이 없습니다.

예를 들어 다음 그릴 수 있는 리소스 디렉터리는 응용 프로그램을 고려 합니다.

    drawable
    drawable-en
    drawable-fr-rCA
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

및 다음 구성 사용 하 여 장치에서 응용 프로그램이 실행 되는 이제:

- **로캘** &ndash; EN-GB
- **방향** &ndash; 포트
- **밀도 화면** &ndash; hdpi
- **터치 스크린이 형식** &ndash; notouch
- **기본 입력된 방법** &ndash; 12key

로캘 내용과 충돌은 프랑스어 리소스는 제거 하는 처음에 `en-GB`, 회원님의 사용:

    drawable
    drawable-en
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

위의 한정자 테이블에서 첫 번째 한정자를 선택 하는 다음으로: MCC 및 MNC 합니다. MCC/MNC 코드는 무시 됩니다 있으므로이 한정자를 포함 하는 리소스 디렉터리가 있습니다.

다음 한정자는 언어는 선택 되어 있습니다. 언어 코드와 일치 하는 리소스가 있습니다. 언어 코드로 일치 하지 않는 모든 리소스 디렉터리 `en` 리소스 목록에는 이제 있도록 거부 됩니다.

    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi

화면 방향을 화면 방향을 일치 하지 않는 모든 리소스 디렉터리에 대 한 존재 하는 다음 한정자가 `port` 제거 됩니다.

    drawable-en-port
    drawable-en-port-ldpi

다음 화면 밀도 대 한 한정자에는 `ldpi`, 더 많은 리소스 디렉터리 하나를 제외 결과:

    drawable-en-port-ldpi

이 과정의 결과로 Android 리소스를 사용 합니다 그릴 수 있는 리소스 디렉터리에 `drawable-en-port-ldpi` 장치에 대 한 합니다.

> [!NOTE]
> 화면 크기 한정자는이 선택 프로세스의 한 가지 예외를 제공합니다. Android 리소스 제공 하는 현재 장치 보다 작은 화면에 대 한 설계를 선택 하는 것이 불가능 합니다. 큰 화면 장치에서 리소스를 사용할 수 있습니다는 예를 들어 기본 크기의 화면을 제공 합니다. 그러나이 반대는 성립 하지 않습니다: 같은 큰 화면 장치 xlarge 화면에 대해 제공 되는 리소스를 사용 하지 것입니다. Android에서 지정 된 화면 크기와 일치 하는 리소스 집합을 찾을 수 없으면 응용 프로그램의 작동이 중단 됩니다.
