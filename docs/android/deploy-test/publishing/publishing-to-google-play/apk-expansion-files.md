---
title: APK 확장 파일
ms.prod: xamarin
ms.assetid: DB7E38E8-3C4E-5191-27EA-22DE63044FE2
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: ccdf1e3fc0c42f8af8f9219a8b472827048a90dc
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69525222"
---
# <a name="apk-expansion-files"></a>APK 확장 파일

일부 애플리케이션(예: 일부 게임)의 경우 Google Play에서 정한 Android 최대 앱 크기 제한에서 제공할 수 있는 것보다 많은 리소스와 자산이 필요합니다. 이 제한은 APK가 대상으로 하는 Android 버전에 다라 다릅니다.

- Android 4.0 이상을 대상으로 하는 APK의 경우 100MB(API 수준 14 이상)
- Android 3.2 이하를 대상으로 하는 APK의 경우 50MB(API 수준 13 이상)

이 제한을 극복하기 위해 Google Play는 APK와 함께 두 *확장 파일*을 호스트 및 배포하여 애플리케이션이 간접적으로 이 제한을 넘을 수 있게 합니다. 

대부분의 디바이스에서 애플리케이션이 설치되면 확장 파일이 APK와 함께 다운로드되며 디바이스의 공유 저장 위치(SD 카드나 USB 마운트 가능 파티션)에 저장됩니다. 일부 구형 디바이스에서는 확장 파일이 APK와 함께 자동으로 설치되지 않을 수 있습니다. 이 경우 애플리케이션 안에 사용자가 최초로 애플리케이션을 실행할 때 확장 파일을 다운로드하는 코드가 포함되어야 합니다.

확장 파일은 *불투명 이진 BLOB(obb)* 으로 간주되며 최대 2GB 크기가 가능합니다. Android는 이러한 파일을 다운로드한 후 특별한 처리를 수행하지 않으며 &ndash;이러한 파일은 애플리케이션에 적합한 어느 형식이나 가능합니다. 개념적으로 확장 파일에 권장되는 방식은 다음과 같습니다.

- **기본 확장** &ndash; 이 파일은 APK 크기 제한에 맞지 않는 리소스 및 자산을 위한 기본 확장 파일입니다. 기본 확장 파일은 애플리케이션에 필요하며 거의 업데이트되지 않는 기본 자산을 포함해야 합니다.
- **패치 확장** &ndash; 기본 확장 파일에 대한 소규모 업데이트용입니다. 이 파일은 업데이트될 수 있습니다. 이 파일에서 필요한 패치나 업데이트를 수행하는 것은 애플리케이션이 담당합니다.


확장 파일은 APK 업로드와 동시에 업로드해야 합니다.
Google Play에서는 기존 APK에 확장 파일을 업로드하거나 기존 APK를 업데이트하는 것은 허용되지 않습니다. 확장 파일 업데이트가 필요한 경우 새 APK를 업데이트된 `versionCode`와 함께 업로드해야 합니다.


## <a name="expansion-file-storage"></a>확장 파일 스토리지

파일이 디바이스에 다운로드되면 **_shared-store_/Android/obb/_package-name_** 에 저장됩니다.

- **_shared-store_** &ndash; `Android.OS.Environment.ExternalStorageDirectory`에서 지정한 디렉터리입니다.
- **_package-name_** &ndash; 애플리케이션의 Java 스타일 패키지 이름입니다.


다운로드 후에는 확장 파일을 디바이스의 위치에서 이동, 변경, 이름 변경 또는 삭제할 수 없습니다. 이렇게 하면 확장 파일을 다시 다운로드하며 구 파일이 삭제됩니다. 또한 확장 파일 디렉터리에는 확장 팩 파일만 포함되어야 합니다.

확장 파일은 콘텐츠에 대한 보안이나 보호를 제공하지 않으므로 &ndash; 다른 애플리케이션이나 사용자가 공유 스토리지에 저장된 모든 파일에 액세스할 수 있습니다.

확장 파일의 압축을 풀어야 할 경우 압축을 푼 파일은 `Android.OS.Environment.ExternalStorageDirectory`의 디렉터리 같은 별도의 디렉터리에 저장해야 합니다.

확장 파일에서 파일 압축을 푸는 또 다른 방법은 확장 파일에서 직접 자산이나 리소스를 읽는 것입니다. 확장 파일은 적합한 `ContentProvider`에 사용할 수 있는 zip 파일 수준입니다. [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary)에 어셈블리 [System.IO.Compression.Zip](https://github.com/mattleibow/Android.Play.ExpansionLibrary/tree/master/System.IO.Compression.Zip)이 포함되며 여기에는 일부 미디어 파일에 대한 직접 액세스를 허용하는 `ContentProvider`가 포함됩니다. 미디어 파일을 zip 파일로 묶으면 미디어 재생 호출에서 zip 파일 압축을 풀지 않고도 직접 zip의 파일을 사용할 수 있습니다. 미디어 파일은 zip 파일에 추가될 때 압축되어서는 안 됩니다. 


### <a name="filename-format"></a>파일 이름 형식

확장 파일을 다운로드하면 Google Play는 다음 구성표를 사용하여 확장 이름을 지정합니다.

```
[main|patch].<expansion-version>.<package-name>.obb
```

이 스키마의 세 가지 구성 요소는 다음과 같습니다.

- `main` 또는 `patch` &ndash; 기본 또는 패치 확장 파일인지 여부를 지정합니다. 각각 하나만 있어야 합니다.
- `<expansion-version>` &ndash; 파일이 처음 연결된 APK의 `versionCode`에 일치하는 정수입니다.
- `<package-name>`&ndash; 애플리케이션의 Java 스타일 패키지 이름입니다.


예를 들어, APK 버전이 21이고 패키지 이름이 `mono.samples.helloworld`이면 기본 확장 파일의 이름은 **main.21.mono.samples.helloworld**가 됩니다.


## <a name="download-process"></a>다운로드 프로세스

Google Play에서 애플리케이션을 설치할 때 확장 파일이 APK와 함께 다운로드 및 저장되어야 합니다. 특정 상황에서 이것이 발생하지 않거나 확장 파일이 삭제될 수 있습니다. 이 문제를 처리하려면 필요한 경우 앱이 확장 파일이 존재하는지 확인한 다음 다운로드해야 합니다. 다음 순서도는 이 프로세스의 권장 워크플로를 나타냅니다.

[![APK 확장 순서도](apk-expansion-files-images/apkexpansion.png)](apk-expansion-files-images/apkexpansion.png#lightbox)

애플리케이션이 시작되면 적합한 확장 파일이 현재 디바이스에 있는지 확인하는 검사를 수행해야 합니다. 없으면 애플리케이션이 Google Play의 [애플리케이션 라이선스](https://developer.android.com/google/play/licensing/index.html)에서 요청해야 합니다. 이 검사는 *LVL(라이선스 확인 라이브러리)* 를 통해 이루어지며 무료 및 라이선스 애플리케이션 모두에 대해 수행되야 합니다. LVL은 주로 유료 애플리케이션에서 라이선스 제한을 적용하기 위해 사용됩니다. 그러나 Google은 확장 라이브러리에도 이를 사용할 수 있게 LVL을 확장하고 있습니다. 무료 애플리케이션은 LVL 검사를 수행해야 하지만 라이선스 제한은 무시할 수 있습니다. LVL 요청은 애플리케이션에서 요구하는 확장 파일에 관한 다음 정보를 제공하기 위한 것입니다. 

- **파일 크기** &ndash; 확장 파일의 파일 크기는 정확한 확장 파일을 이미 다운로드했는지 여부를 판단하기 위한 검사의 일부로 사용됩니다.
- **파일 이름**&ndash; 확장 팩을 저장해야 하는 파일 이름(현재 디바이스의)입니다.
- **다운로드 URL** &ndash; 확장 팩을 다운로드하는 데 사용하는 URL입니다. 다운로드마다 고유하며 제공 직후 만료됩니다.


LVL 검사가 수행된 후 애플리케이션은 확장 파일을 다운로드하게 되는데 다운로드의 일부로 다음 사항을 고려합니다.

- 디바이스에 확장 파일 저장을 위한 공간이 부족할 수 있습니다.
- Wi-Fi를 사용할 수 없는 경우 사용자는 원치 않는 데이터 요금이 발생하지 않게 다운로드를 일시 중지하거나 취소할 수 있어야 합니다.
- 확장 파일은 사용자 작업을 차단하지 않기 위해 백그라운드에서 다운로드됩니다.
- 다운로드가 백그라운드에서 수행되는 동안 진행률 표시기가 나타나야 합니다.
- 다운로드하는 동안 발생하는 오류는 정상적으로 처리되며 복구할 수 있습니다.



## <a name="architectural-overview"></a>아키텍처 개요

기본 활동이 시작되면 확장 파일이 다운로드되었는지 검사합니다. 파일이 다운로드된 경우 파일에서 유효성을 검사합니다.

확장 파일을 다운로드하지 않았거나 현재 파일이 무효한 경우 새 확장 파일을 다운로드해야 합니다. 바인딩된 서비스가 애플리케이션의 일부로 만들어집니다. 애플리케이션의 기본 활동이 시작되면 바인딩된 서비스를 통해 Google 라이선스 서비스에 대한 검사를 수행하여 확장 파일 이름과 다운로드할 파일의 URL을 확인합니다. 그런 다음 바인딩된 서비스가 백그라운드 스레드에 파일을 다운로드합니다.

확장 파일을 애플리케이션에 통합하는 데 필요한 작업을 간편하게 하기 위해 Google에서는 몇 가지 Java 라이브러리를 제작했습니다. 이 라이브러리는 다음과 같습니다.

- **Downloader 라이브러리**&ndash; 애플리케이션에서 확장 파일을 통합하는 데 필요한 작업을 줄여 주는 라이브러리입니다. 이 라이브러리는 백그라운드 서비스에 확장 파일을 다운로드하고, 사용자 알림을 표시하며, 네트워크 연결 문제를 처리하고, 다운로드를 재개하는 등의 작업을 수행합니다.
- **LVL(라이선스 확인 라이브러리)** &ndash; 애플리케이션 라이선스 서비스에 대한 호출을 수행 및 처리하기 위한 라이브러리입니다. 해당 디바이스에서 애플리케이션의 사용 권한이 있는지 확인하기 위한 라이선스 검사를 수행하는 데도 사용됩니다.
- **APK 확장 Zip 라이브러리(선택 사항)** &ndash; 확장 파일이 zip 파일 형식인 경우 이 라이브러리가 콘텐츠 공급자 역할을 하며 애플리케이션이 zip 파일 확장 없이 zip 파일에서 직접 리소스와 자산을 읽을 수 있게 합니다.


이 라이브러리는 C#에 이식되었으며 Apache 2.0 라이선스에서 사용할 수 있습니다. 확장 파일을 기존 애플리케이션에 신속하게 통합할 수 있게 이 라이브러리를 기존 Xamarin.Android 애플리케이션에 추가할 수 있습니다. 이 코드는 GitHub의 [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary)에서 제공합니다.
