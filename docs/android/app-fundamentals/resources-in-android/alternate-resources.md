---
title: 대체 리소스
ms.prod: xamarin
ms.assetid: AE5A864E-192D-475E-C731-99249C2E7D9E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
ms.openlocfilehash: e71e79b58d912ecb697576e92ae921a848f24f4c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013388"
---
# <a name="alternate-resources"></a>대체 리소스

대체 리소스는 특정 장치 또는 픽셀 밀도, 특정 화면 크기, 또는 현재 언어와 같은 런타임 구성을 대상으로 하는 리소스입니다. Android에는 특정 장치 또는 구성에 대 한 보다 구체적인 기본 리소스는 리소스를 일치 시킬 수 있습니다, 한 리소스를 대신 사용 됩니다. 현재 구성과 일치 하는 대체 리소스를 찾지 못하면 기본 리소스가 로드 됩니다. 리소스 위치 섹션에서 자세히 아래에서 응용 프로그램에서 사용할 리소스를 설명 합니다 Android 결정 하는 방법

대체 리소스는 기본 리소스와 마찬가지로 리소스 형식에 따라 리소스 폴더 안에 하위 디렉터리로 구성 됩니다. 대체 리소스 하위 디렉터리의 이름이 형식: _ResourceType_-_Qualifier_

*한정자* 특정 장치 구성을 식별 하는 이름입니다.
각 대시로 구분 된 이름에 둘 이상의 한정자 있을 수 있습니다. 예를 들어 아래 스크린샷에서 로캘, 화면 밀도, 화면 크기, 방향 등 다양 한 구성에 대 한 대체 리소스가 있는 간단한 프로젝트를 보여 줍니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![대체 리소스](alternate-resources-images/alternate-resources-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![대체 리소스](alternate-resources-images/alternate-resources-xs.png)
 
-----
 

리소스 형식 한정자를 추가 하는 경우 다음 규칙에 적용 됩니다.

1. 대시로 구분 된 각 한정자를 사용 하 여 둘 이상의 한정자가 있을 수 있습니다.

2. 한정자가 한 번만 지정할 수 있습니다.

3. 한정자는 아래 표에 표시 되는 순서 대로 여야 합니다.

참조에 대 한 가능한 한정자는 아래와 같습니다.

- **MCC와 MNC** &ndash; 는 [모바일 국가 코드](https://en.wikipedia.org/wiki/List_of_mobile_country_codes) (MCC) 및 필요에 따라 합니다 [모바일 네트워크 코드](https://en.wikipedia.org/wiki/Mobile_Network_Code) (MNC). SIM 카드 장치를 연결할 네트워크를 MNC 제공 하는 동안 MCC를 제공 합니다. 대상 로캘로 모바일 국가 코드를 사용 하 여 가능한 경우에 권장 방법은 아래에 지정 된 언어 한정자를 사용 하는 것입니다. 예를 들어, 대상 리소스 독일에 한정자 것 `mcc262`입니다. 대상 리소스 미국의 T Mobile에 대 한 한정자가 `mcc310-mnc026`합니다.
  모바일 국가 코드 및 모바일 네트워크 코드의 전체 목록을 참조 하세요. <http://mcc-mnc.com/>합니다.

- **언어** &ndash; 2 자로 [ISO 639-1 언어 코드](https://en.wikipedia.org/wiki/ISO_639-1) 및 필요에 따라 두 문자 뒤 [ISO 3166-알파 2 지역 코드](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)합니다. 
  으로 구분 하는 경우 두 한정자가 제공 되는 `-r`합니다. 대상 로캘을 프랑스어의 한정자를 예를 들어 `fr` 사용 됩니다. 캐나다 로캘을 대상으로 `fr-rCA` 사용 됩니다. 언어 코드 및 지역 코드의 전체 목록은 참조 하세요 [표시의 이름이의 언어에 대 한 코드](http://www.loc.gov/standards/iso639-2/php/English_list.php) 하 고 [국가 이름 및 코드 요소](http://www.iso.org/iso/country_codes/iso_3166_code_lists/country_names_and_code_elements.htm)합니다.

- **최소 너비** &ndash; 응용 프로그램에서 실행 하려는 가장 작은 화면 너비를 지정 합니다. 더 자세하게 다룹니다 [다양 한 화면에 대 한 리소스를 만드는](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)합니다. 
  API 수준 (Android 3.2) 13 이상에 사용할 수 있습니다. 예를 들어, 한정자 `sw320dp` 해당 높이 너비 이상이 대상 장치에는 320dp 합니다.

- **사용 가능한 너비** &ndash; 화면에 형식 w의 최소 너비*N*dp를 여기서 *N* 독립적 픽셀 밀도의 너비입니다.
  이 값은 사용자가 장치를 회전할 때 변경 될 수 있습니다. 더 자세하게 다룹니다 [다양 한 화면에 대 한 리소스를 만드는](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)합니다. 
  API 수준 (Android 3.2) 13 이상에 사용할 수 있습니다. 예: 한정자 w720dp 최소 720dp 너비가 있는 장치를 대상으로 하는 데 사용 됩니다.

- **높이** &ndash; 화면에 형식 h의 최소 높이*N*dp, 여기서 *N* dp의 높이입니다. 이 값은 사용자가 장치를 회전할 때 변경 될 수 있습니다. 더 자세하게 다룹니다 [다양 한 화면에 대 한 리소스를 만드는](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)합니다. 
  API 수준 (Android 3.2) 13 이상에 사용할 수 있습니다. 예를 들어, 한정자 h720dp 최소 720dp 높이가 있는 장치를 대상으로 사용 됩니다.

- **화면 크기** &ndash; 이 한정자가 있는 이러한 리소스에 대 한 화면 크기의 일반화 합니다. 더 자세하게에서 다룹니다 [다양 한 화면에 대 한 리소스를 만드는](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)합니다. 
  가능한 값은 `small`, `normal`, `large` 및 `xlarge`입니다. API 수준 (Android 2.3/Android 2.3.1/Android 2.3.2) 9에 추가

- **화면 가로 세로** &ndash; 를 기반으로 가로 세로 비율을 화면 방향이 없습니다. 긴 화면이 큽니다. API 수준 (Android 1.6) 4에에서 추가 합니다. 가능한 값은 long 및 notlong입니다.

- **화면 방향** &ndash; 세로 또는 가로 화면 방향입니다. 이 응용 프로그램의 수명 동안 변경할 수 있습니다.
  가능한 값은 `port`와 `land`입니다.

- **모드를 도킹** &ndash; 자동차의 장치 도킹 하거나 도킹 책상에 대 한 합니다. API 수준 8 (이상 Android (2.2.x)에 추가 합니다. 가능한 값은 `car`와 `desk`입니다.

- **야간 모드** &ndash; 응용 프로그램은 밤 또는 날에 실행 여부. 이 응용 프로그램의 수명 동안 변경 될 수 있으며 개발자에 게 밤 인터페이스의 음영이 짙을 수록 버전을 사용 하는 기회를 제공 하려는 합니다. API 수준 8 (이상 Android (2.2.x)에 추가 합니다. 가능한 값은 `night`와 `notnight`입니다.

- **픽셀 밀도 (dpi)를 화면** &ndash; 실제 화면에 지정된 된 영역에서 픽셀 수입니다. 일반적으로 dpi (인치당)으로 표현 합니다. 가능한 값은 다음과 같습니다.

    - `ldpi` &ndash; 화면 밀도 낮습니다.

    - `mdpi` &ndash; 보통 밀도 화면

    - `hdpi` &ndash; 밀도가 높은 화면

    - `xhdpi` &ndash; 매우 높은 밀도 화면

    - `nodpi` &ndash; 확장 될 필요가 있는 리소스

    - `tvdpi` &ndash; API 수준 (Android 3.2) 13 화면에 대 한 hdpi mdpi 사이에서 도입 되었습니다.

- **터치 스크린 형식** &ndash; 터치 스크린 장치에서 사용할 수 있습니다의 유형을 지정 합니다. 가능한 값은 `notouch` (터치 스크린 없음), `stylus` (저항 식 원하는 터치 스크린과 스타일러스 적합) 및 `finger` (터치 스크린).

- **가용성 키보드** &ndash; 키보드의 종류를 사용할 수를 지정 합니다. 응용 프로그램의 수명 동안 변경 될 수 있습니다이 &ndash; 사용자 하드웨어 키보드를가 하는 경우에 예를 들어 있습니다. 가능한 값은 다음과 같습니다.

    - `keysexposed` &ndash; 장치에는 바로 사용할 수 있습니다. 사용할 수 없는 소프트웨어 키보드의 경우 다음이 사용 됩니다 하드웨어 키보드를 열 때.

    - `keyshidden` &ndash; 장치 하드웨어 키보드가 있는지 않습니다 하지만 숨겨져 하 고 소프트웨어 키보드가 활성화 됩니다.

    - `keyssoft` &ndash; 장치에 소프트웨어 키보드 사용 하도록 설정 합니다.

- **기본 텍스트 입력 방법** &ndash; 사용 하 여 어떤 종류의 하드웨어 키 입력에 사용할 수를 지정 합니다. 가능한 값은 다음과 같습니다.

    - `nokeys` &ndash; 입력에 대 한 하드웨어 키가 있습니다.

    - `qwerty` &ndash; Qwerty 키보드는 사용할 수 있습니다.

    - `12key` &ndash; 12 키 하드웨어 키보드는


- **탐색 키 가용성** &ndash; 5 양방향 또는 방향 패드 (패드 방향) 탐색 사용 가능한 경우에 대 한 합니다. 이 응용 프로그램의 수명 동안 변경할 수 있습니다. 가능한 값은 다음과 같습니다.

    - `navexposed` &ndash; 탐색 키를 사용자에 게 제공 됩니다.

    - `navhidden` &ndash; 탐색 키를 사용할 수 없는 경우

-  **비-터치 탐색 주로** &ndash; 장치에서 사용할 수 있는 탐색의 종류입니다. 가능한 값은 다음과 같습니다.

    - `nonav` &ndash; 사용할 수 있는 탐색 기능은 터치 스크린

    - `dpad` &ndash; 탐색 방향 패드 (패드 방향) 수

    - `trackball` &ndash; 장치에서 트랙볼 탐색

    - `wheel` &ndash; 개 하거나 자세한 방향 바퀴 사용할 수 있는 일반적인 시나리오

-  **플랫폼 버전 (API 수준)** &ndash; v 형식으로 장치에서 지원 되는 API 수준*N*여기서 *N* 대상이 되는 API 수준입니다. 예를 들어 v11 대상 API 수준 11 (Android 3.0) 장치.


한정자가 리소스에 대 한 자세한 내용은 참조 [리소스 제공](https://developer.android.com/guide/topics/resources/providing-resources.html) Android 개발자 웹 사이트입니다.


## <a name="how-android-determines-what-resources-to-use"></a>Android에서 사용할 리소스를 결정 하는 방법

것이 가능 하 고 Android 응용 프로그램 리소스가 많이 들어 있습니다. 해당 것을 알고 있어야 하는 방법을 통해 Android 장치에서 실행 될 때 응용 프로그램에 대 한 리소스를 선택 합니다.

규칙의 다음 테스트를 반복 하 여 기본 리소스를 결정 하는 android:

- **모순 한정자를 제거** &ndash; 예를 들어 장치 방향을 세로 이면 다음 모든 가로 리소스 디렉터리 거부 됩니다.

- **지원 되지 않습니다 한정자를 무시** &ndash; 모든 한정자는 모든 API 수준에 사용할 수 있습니다. 리소스 디렉터리에 있는 경우 장치에서 지원 되지 않는 한정자, 해당 리소스 디렉터리 무시 됩니다.

- **다음 가장 높은 우선 순위 한정자를 식별** &ndash; 맨 위에서 아래로 다음 가장 높은 우선 순위 한정자의 선택을 위 테이블을 참조 합니다.

- **한정자에 대 한 모든 리소스 디렉터리를 유지** &ndash; 경우 위의 테이블에 한정자와 일치 하는 모든 리소스 디렉터리 (위에서 아래로) 다음 가장 높은 우선 순위 한정자를 선택 합니다.

이러한 규칙은 다음 순서도에 나와 있습니다.

[![리소스 순서도](alternate-resources-images/flowchart-sml.png)](alternate-resources-images/flowchart.png#lightbox)

시스템 밀도 별 리소스를 찾고를 찾을 수 없습니다. 다른 밀도 특정 리소스를 찾도록 하 고 확장할 수 시도 합니다. Android는 기본 리소스를 반드시 사용 하지 않을 수 있습니다.
예를 들어 저밀도 리소스 및이 사용할 수 없는 Android 고밀도 버전 리소스의 기본 또는 중간 밀도 리소스에 대해 선택할 수 있습니다. 이 있기 때문 고밀도 리소스 규모를 축소 0.75의 비율을 해야 하는 중간 밀도 리소스 보다 더 적은 가시성 문제가 됩니다. 0.5의 비율로 축소할 수 있습니다.

예를 들어 다음 드로어 블 리소스 디렉터리에 있는 응용 프로그램을 고려 합니다.

    drawable
    drawable-en
    drawable-fr-rCA
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

다음 구성 사용 하 여 장치에서 응용 프로그램은 실행 하는 이제 하합니다.

- **로캘** &ndash; EN-GB
- **방향을** &ndash; 포트
- **화면 밀도** &ndash; hdpi
- **터치 스크린 형식** &ndash; notouch
- **기본 입력된 방법을** &ndash; 12key

로캘을 사용 하 여 충돌 프랑스어 리소스를 먼저 제거 됩니다 `en-GB`를 사용 하 여 남습니다.

    drawable
    drawable-en
    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi
    drawable-port-ldpi
    drawable-port-notouch-12key

다음으로, 첫 번째 한정자는 위 한정자가 테이블에서 선택 됩니다. MCC와 MNC 합니다. MCC/MNC 코드는 무시 됩니다 있도록이 한정자를 포함 하는 리소스 디렉터리가 있습니다.

다음 한정자는 언어가 선택 되었습니다. 언어 코드와 일치 하는 리소스가 있습니다. 언어 코드와 일치 하지 않는 모든 리소스 디렉터리 `en` 리소스 목록이 이제 되도록 거부 됩니다.

    drawable-en-port
    drawable-en-notouch-12key
    drawable-en-port-ldpi

화면 방향, 화면 방향을 일치 하지 않는 모든 리소스 디렉터리에 대해 존재 하는 다음 한정자가 `port` 제거 됩니다.

    drawable-en-port
    drawable-en-port-ldpi

다음은 화면 밀도 대 한 한정자 `ldpi`, 그러면 하나의 제외 자세한 리소스 디렉터리:

    drawable-en-port-ldpi

이 프로세스의 결과로 Android 사용할지 드로어 블 리소스 리소스 디렉터리에 `drawable-en-port-ldpi` 장치에 대 한 합니다.

> [!NOTE]
> 화면 크기 한정자는이 선택 프로세스의 한 가지 예외를 제공합니다. 현재 어떤 장치를 제공 하는 보다 작은 화면에 대 한 설계 된 리소스를 선택 하는 Android에 대 한 것 같습니다. 큰 화면 장치에서 리소스를 사용할 수는 예를 들어, 일반적인 크기의 화면을 제공 합니다. 하지만이도 마찬가지 없습니다: 같은 큰 화면 장치는 초대형 화면에 제공 되는 리소스를 사용 하지 것입니다. Android에서 지정 된 화면 크기와 일치 하는 리소스 집합을 찾지 못하면 응용 프로그램 작동이 중단 됩니다.
