---
title: 마모 앱 패키징
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: af96c0f8cf862b7a208beb5b91ecbb30598b09d9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="packaging-wear-apps"></a>마모 앱 패키징

Google Play에서 배포에 대 한 전체 Android 앱과 android 마모 앱 패키지 됩니다. 

## <a name="automatic-packaging"></a>자동 패키징

Xamarin Android 5.0 이상에서는 마모 앱은 자동으로로 패키지 핸드헬드 앱의 리소스 핸드헬드 프로젝트에서 마모 프로젝트에 대 한 프로젝트 참조를 만들 때. 이러한 연결을 만드는 다음 단계를 사용할 수 있습니다. 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 마모 앱이 휴대용 솔루션의 일부로 이미 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 기존 프로젝트 추가...** .

2. 로 이동 된 **.csproj** 파일 마모 응용 프로그램의 선택 하 고 클릭 **열려**합니다. 이제 마모 응용 프로그램 프로젝트 핸드헬드 솔루션에 표시 됩니다.

3. 마우스 오른쪽 단추로 클릭는 **참조** 노드 선택한 **참조 추가**합니다.

4. 에 **참조 관리자** 대화 상자, 마모 프로젝트 (확인 표시를 추가 하려면 클릭)를 클릭 한 다음 활성화 **확인**합니다.

5. 핸드헬드 프로젝트의 패키지 이름을 일치 하도록 마모 프로젝트에 대 한 패키지 이름을 변경 (패키지 이름 아래에서 변경할 수 있습니다 **속성 > Android 매니페스트**).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 마모 앱이 휴대용 솔루션의 일부로 이미 솔루션 노드를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 기존 프로젝트 추가...** .

2. 로 이동 된 **.csproj** 파일 마모 응용 프로그램의 선택 하 고 클릭 **열려**합니다. 이제 마모 응용 프로그램 프로젝트 핸드헬드 솔루션에 표시 됩니다.

3. 솔루션 및 클릭에서 핸드헬드 프로젝트 노드를 마우스 오른쪽 단추로 클릭 **참조 편집...** .

4. 에 **참조 편집** 대화 상자, 마모 프로젝트 (확인 표시를 추가 하려면 클릭)를 클릭 한 다음 활성화 **확인**합니다.

5. 핸드헬드 프로젝트의 패키지 이름을 일치 하도록 마모 프로젝트에 대 한 패키지 이름을 변경 (패키지 이름 아래에서 변경할 수 있습니다 **프로젝트 옵션 > Android 응용 프로그램**).

-----


받아볼 수는 **XA5211** 마모 앱의 패키지 이름을 핸드헬드 응용 프로그램의 패키지 이름과 일치 하지 않는 경우 오류가 발생 합니다. 예를 들어:

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

이 오류를 해결 하려면 핸드헬드 앱의 패키지 이름을 일치 하도록 마모 응용 프로그램의 패키지 이름을 변경 합니다.

클릭할 때 **빌드 > 모든 빌드**,이 연결을 주 Handheld (전화) 프로젝트에 자동 패키징 마모 프로젝트의 트리거합니다. 자동으로 마모 앱이 빌드되고 핸드헬드 응용 프로그램에서 리소스로 포함 됩니다.

마모 응용 프로그램 프로젝트를 생성 하는 어셈블리 Handheld (전화) 프로젝트의 어셈블리 참조로 사용 되지 않습니다. 대신, 빌드 프로세스는 다음을 수행합니다.

-   패키지 이름이 일치 하는지 확인 합니다. 

-   XML을 생성 하 고 마모 앱과 연결할 핸드헬드 프로젝트에 추가 합니다. 예를 들어: 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

-   로 마모 앱이 추가 **원시** 핸드헬드 프로젝트에 리소스입니다. 


## <a name="manual-packaging"></a>수동 패키징

이전 버전 5.0, Xamarin.Android에 쓰는 유형 Android 앱을 작성할 수 있습니다 하지만 앱을 배포 하려면 다음 수동 패키징 지침을 따라야 합니다. 

1. 착용 식 프로젝트와 Handheld (전화) 프로젝트는 동일한 버전 번호와 패키지 이름을 가졌는지 확인 합니다.

2. 수동으로으로 착용 식 프로젝트를 빌드는 **릴리스** 작성 합니다.

3. 릴리스를 수동으로 추가 **합니다. APK** 단계의 (2)에 **리소스/원시** Handheld (전화) 프로젝트의 디렉터리입니다.

4. 새 XML 리소스를 수동으로 추가 **Resources/xml/wearable_app_desc.xml** 착용 식에 가리키는 핸드헬드 프로젝트에서 **APK** 단계 (3)에서:

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. 수동으로 추가 `<meta-data />` 요소를 핸드헬드 프로젝트 **AndroidManifest.xml** `<application>` 새 XML 리소스를 참조 하는 요소:

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

Android 개발자 사이트의를 참조 하십시오. [수동 packging 지침](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually)합니다.

