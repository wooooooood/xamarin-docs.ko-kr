---
제목: "Android 빌드 오류 – LinkAssemblies 작업이 예기치 않게 실패 했습니다.". 토픽: 문제 해결: assetid: EB3BE685-CB72-48E3-89D7-C845E76B9FA2: xamarin-forms author: davidbritch m. author: dabritch. 날짜: 03/07/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="android-build-error--the-linkassemblies-task-failed-unexpectedly"></a>Android 빌드 오류 – LinkAssemblies 작업이 예기치 않게 실패 했습니다.

`The "LinkAssemblies" task failed unexpectedly`양식을 사용 하는 Xamarin Android 프로젝트를 빌드할 때 오류 메시지가 표시 될 수 있습니다. 이는 링커가 활성 상태일 때 (일반적으로 *릴리스* 빌드에서 앱 패키지의 크기를 줄이기 위해) 발생 합니다. Android 대상이 최신 프레임 워크로 업데이트 되지 않기 때문에 발생 합니다. (추가 정보: [ Xamarin.Forms 지원 되는 플랫폼](~/get-started/supported-platforms.md#android-platform-support))

이 문제를 해결 하려면 지원 되는 최신 Android SDK 버전이 있는지 확인 하 고 **대상 프레임 워크** 를 설치 된 최신 플랫폼으로 설정 합니다. 또한 **대상 Android 버전** 을 설치 된 최신 플랫폼으로 설정 하 고 **최소 ANDROID 버전** 을 API 19 이상으로 설정 하는 것이 좋습니다. 이는 지원 되는 구성으로 간주 됩니다.

## <a name="setting-in-visual-studio-for-mac"></a>Mac용 Visual Studio에서 설정

1. Android 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 메뉴에서 **옵션** 을 선택 합니다.
2. **프로젝트 옵션** 대화 상자에서 **빌드 > 일반**으로 이동 합니다.
3. **Android 버전을 사용 하 여 컴파일: (대상 프레임 워크)** 를 설치 된 최신 플랫폼으로 설정 합니다.
4. **프로젝트 옵션** 대화 상자에서 **빌드 > Android 응용 프로그램**으로 이동 합니다.
5. **최소 android 버전** 을 API 수준 19 이상으로 설정 하 고 **대상 android 버전** 을 (3)에서 선택한 최신 플랫폼으로 설정 합니다.

## <a name="setting-in-visual-studio"></a>Visual Studio에서 설정

1. Android 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 메뉴에서 **속성** 를 선택 합니다.
2. 프로젝트 속성에서 **응용 프로그램**으로 이동 합니다.
3. **Android 버전을 사용 하 여 컴파일: (대상 프레임 워크)** 를 설치 된 최신 플랫폼으로 설정 합니다.
4. 프로젝트 속성에서 **Android 매니페스트**로 이동 합니다.
5. **최소 android 버전** 을 API 수준 19 이상으로 설정 하 고 **대상 android 버전** 을 (3)에서 선택한 최신 플랫폼으로 설정 합니다.

이러한 설정을 업데이트 한 후에는 프로젝트를 정리 하 고 다시 빌드하여 변경 내용이 선택 되었는지 확인 하세요.
