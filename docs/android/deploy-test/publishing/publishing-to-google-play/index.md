---
title: Google Play에 게시
ms.prod: xamarin
ms.assetid: FB1CC234-3554-8566-48BD-2B9B3A28CC7F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
---

# <a name="publishing-to-google-play"></a>Google Play에 게시

앱을 배포할 수 있는 여러 앱 마켓이 있지만 Google Play가 당연히 세게에서 가장 크고 많은 사용자들이 찾는 Android 앱 스토어입니다. Google Play는 Android 애플리케이션의 배포, 광고, 판매 및 판매 분석을 위한 단일 플랫폼을 제공합니다. 

이 섹션에서는 게시자가 되기 위한 등록, Google Play의 애플리케이션 판촉 및 광고를 위한 자산 수집, Google Play 애플리케이션 등급 지침, 필터를 사용하여 특정 장치에 대한 애플리케이션 배포 제한 등과 같은 Google Play 특정 항목을 다룹니다.


## <a name="requirements"></a>요구 사항

Google Play를 통해 애플리케이션을 배포하려면 개발자 계정을 만들어야 합니다. 이 작업은 한 번만 수행하면 되며 $25 USD를 한 번만 지불하면 됩니다.

모든 애플리케이션은 2033년 10월 22일 이후 만료되는 암호화 키로 서명해야 합니다.

Google Play에 게시하는 APK의 최대 크기는 100MB입니다. 애플리케이션이 이 크기를 초과할 경우 Google Play에서는 *APK 확장 파일*을 통한 추가 자산의 제공을 허용합니다. Android 확장 파일에서는 APK에 추가 파일 2개를 허용하며 각 파일은 최대 2GB 크기까지 가능합니다. Google Play는 무료로 이 파일을 호스트 및 배포합니다. 확장 파일은 다른 섹션에서 설명합니다.

Google Play를 전세계에서 사용할 수 있는 것은 아닙니다. 일부 위치에서는 애플리케이션 배포가 지원되지 않을 수 있습니다.



## <a name="becoming-a-publisher"></a>게시자 되기

Google play에서 애플리케이션을 게시하려면 게시자 계정이 있어야 합니다. 게시자 계정에 등록하려면 다음 단계를 따릅니다.

1.  [Google Play 개발자 콘솔](https://play.google.com/apps/publish)을 방문합니다.
1.  개발자 ID에 대한 기본 정보를 입력합니다.
1.  로캘의 개발자 배포 규약을 읽고 규약에 동의합니다.
1.  $25 USD 등록 요금을 지불합니다.
1.  이메일을 통해 확인합니다.
1.  계정이 만들어진 후 Google Play를 사용하여 애플리케이션을 게시할 수 있습니다.


Google Play가 전 세계의 모든 국가를 지원하지는 않습니다. 가장 최신 국가 목록은 다음 링크에서 제공합니다.

1.  [지원되는 개발자 위치 &amp; 판매자 등록](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=150324)&ndash; 개발자가 판매자로 등록하고 유료 애플리케이션을 판매할 수 있는 모든 국가의 목록입니다.

1.  [Google Play 사용자에 대한 배포 지원 위치](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=138294)&ndash; 애플리케이션을 배포할 수 있는 모든 국가의 목록입니다.



### <a name="preparing-promotional-assets"></a>프로모션 자산 준비

Google Play에서 애플리케이션을 효과적으로 프로모션 및 광고할 수 있게 Google에서는 개발자들이 스크린 샷, 그래픽, 비디오 같은 프로모션 자산을 제출할 수 잇게 허용하고 있습니다. 그러면 Google Play가 해당 자산을 사용하여 애플리케이션을 광고 및 프로모션합니다.



#### <a name="launcher-icons"></a>시작 아이콘

*시작 아이콘*은 애플리케이션을 나타내는 그래픽입니다. 각 시작 아이콘은 투명 알파 채널이 있는 32비트 PNG여야 합니다. 애플리케이션에는 아래 목록에서 설명한 대로 모든 일반 화면 밀도에 대한 아이콘이 있어야 합니다. 

-   **ldpi** (120dpi) &ndash; 36 x 36 px
-   **mdpi** (160dpi) &ndash; 48 x 48 px
-   **hdpi** (240dpi) &ndash; 72 x 72 px
-   **xhdpi** (320dpi) &ndash; 96 x 96 px


시작 아이콘은 사용자가 Google Play에서 처음으로 보게 되는 애플리케이션의 모습이므로 시각적으로 의미 있고 매력적인 아이콘이 되도록 주의가 필요합니다.

시작 아이콘 팁:

1.  **간단하고 깔끔할 것** &ndash; 시작 아이콘은 간단하고 깔끔해야 합니다. 즉 애플리케이션의 이름은 아이콘에서 제외합니다. 간단할수록 더 기억에 남고 더 작을수록 구분하기 쉽습니다.

1.  **옅지 않은 아이콘** &ndash; 지나치게 옅은 아이콘은 일부 배경에서 잘 드러나지 않습니다.

1.  **알파 채널 사용** &ndash; 아이콘은 전체 프레임 이미지가 아니면서 알파 채널을 사용해야 합니다.



#### <a name="high-resolution-application-icons"></a>고해상도 애플리케이션 아이콘

Google Play의 애플리케이션에는 충실도 높은 애플리케이션 아이콘 버전이 필요합니다. Google Play에서만 사용되며 애플리케이션 실행 아이콘을 대신하지 않습니다. 고해상도 아이콘의 사양은 다음과 같습니다.

1.  알파 채널 32비트 PNG
1.  512 x 512픽셀
1.  최대 크기 1024KB

[Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/)는 적합한 실행 아이콘과 고해상도 애플리케이션 아이콘을 만드는 데 유용한 도구입니다.



#### <a name="screen-shots"></a>스크린 샷

Google Play에서는 한 애플리케이션에 최소 2개, 최대 8개의 스크린 샷을 요구합니다. 이 스크린 샷은 Google Play에서 애플리케이션의 세부 정보 페이지에 표시됩니다.

스크린 샷의 사양은 다음과 같습니다.

1.  알파 채널이 없는 24비트 JPG 또는 PNG
1.  320w x 480h, 480w x 800h 또는 480w x 854h 가로 이미지는 잘립니다.



#### <a name="promotional-graphic"></a>프로모션 그래픽

Google Play에서 사용하는 선택적 이미지입니다.

1.  알파 채널이 없는 180w x 120h 24비트 PNG 또는 JPG
1.  아트에 테두리 없음



#### <a name="feature-graphic"></a>추천 그래픽

Google Play의 추천 섹션에서 사용합니다. 이 그래픽은 애플리케이션 아이콘 없이 단독으로 표시될 수 있습니다.

1.  알파 채널과 투명도 없는 1024w x 500h PNG 또는 JPG
1.  모든 중요한 콘텐츠는 924 x 500 프레임 안에 있어야 합니다. 이 프레임 외부의 픽셀은 스타일을 위해 잘릴 수 있습니다.
1.  이 그래픽은 축소될 수 있으므로 큰 텍스트를 사용하고 그래픽은 간단하게 유지합니다.



#### <a name="video-link"></a>비디오 링크

애플리케이션을 보여 주는 YouTube 동영상 URL입니다. 비디오는 30초에서 2분 길이까지 가능하며 애플리케이션의 장점을 보여 줍니다.



### <a name="publishing-to-google-play"></a>Google Play에 게시

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin Android 7.0에는 Visual Studio에서 Google Play에 앱을 게시하는 통합 워크플로가 도입되었습니다. Xamarin Android 7.0 이전 버전을 사용하는 경우 Google Play 개발자 콘솔을 통해 APK를 수동으로 업로드해야 합니다. 또한 통합 워크플로를 사용하려면 먼저 하나 이상의 APK가 기존에 업로드되어 있어야 합니다. 첫 번째 APK를 업로드하지 않은 경우 수동으로 업로드해야 합니다. 자세한 내용은 [수동으로 APK 업로드](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md)를 참조하세요.

[새 인증서 만들기](~/android/deploy-test/signing/index.md#newcert)에서는 Android 앱 서명을 위해 새 인증서를 만드는 방법을 설명합니다. 다음 단계는 Google Play에 서명된 된 앱을 게시하는 것입니다.

1. Google Play 개발자 계정에 로그인하여 Google Play 개발자 계정에 연결된 새 프로젝트를 만듭니다.
2. 앱을 인증하는 **OAuth 클라이언트**를 만듭니다.
3. 만든 클라이언트 ID와 클라이언트 암호를 Visual Studio에 입력합니다.
4. Visual Studio에 계정을 등록합니다.
5. 인증서로 앱을 서명합니다.
6. Google Play에 서명된 앱을 게시합니다.

[게시를 위해 보관](~/android/deploy-test/release-prep/index.md#archive)에서 **배포 채널** 대화 상자에는 두 가지 배포 옵션이 제공됩니다. **Ad Hoc** 및 **Google Play**. 이와 다르게 **서명 ID** 대화 상자가 표시되면 **뒤로**를 클릭하여 **배포 채널** 대화 상자로 돌아갑니다. **Google Play**를 선택하고 **다음**을 클릭합니다.

[![배포 채널 대화 상자](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

**서명 ID** 대화 상자에서 [새 인증서 만들기](~/android/deploy-test/signing/index.md#newcert)에서 만든 ID를 선택하고 **계속**을 클릭합니다.

[![서명 ID 대화 상자](images/vs/02-select-identity-sml.png)](images/vs/02-select-identity.png#lightbox)

**Google Play 계정** 대화 상자에서 **+** 단추를 클릭하여 새 Google Play 계정을 추가합니다.

[![Google Play 계정 대화 상자](images/vs/03-google-play-accounts-sml.png)](images/vs/03-google-play-accounts.png#lightbox)

**Google API 액세스 등록** 대화 상자에서 Google Play 개발자 계정에 API 액세스를 제공하는 _클라이언트 ID_와 _클라이언트 암호_를 입력해야 합니다.

[![Google API 액세스 등록 대화 상자](images/vs/04-register-google-api-access-sml.png)](images/vs/04-register-google-api-access.png#lightbox)

다음 섹션에서는 새 Google API 프로젝트를 만들고 필요한 _클라이언트 ID_ 및 _클라이언트 암호_를 생성하는 방법에 대해 설명합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac에는 Google Play에 앱을 게시하는 통합 워크플로가 있습니다.

[새 인증서 만들기](~/android/deploy-test/signing/index.md#newcert)에서는 Android 앱 서명을 위해 새 인증서를 만드는 방법을 설명합니다. 다음 단계에서는 Google Play에 Xamarin.Android 앱을 게시하는 방법을 설명합니다.

1. Google Play 개발자 계정에 로그인하여 Google Play 개발자 계정에 연결된 새 프로젝트를 만듭니다.
2. 앱을 인증하는 _OAuth 클라이언트_를 만듭니다.
3. 만든 _클라이언트 ID_와 _클라이언트 암호_를 Visual Studio for Mac에 입력합니다.
4. Visual Studio for Mac에 계정을 등록합니다.
5. 인증서로 애플리케이션을 서명합니다.
6. Google Play에 서명된 애플리케이션을 게시합니다.

[게시를 위해 보관](~/android/deploy-test/release-prep/index.md#archive)에서 **서명 및 배포...** 대화 상자에는 두 가지 배포 선택 항목이 제공됩니다. **Google Play**를 선택하고 **다음**을 클릭합니다.

[![Android 배포 선택 대화 상자](images/xs/01-select-google-play-sml.png)](images/xs/01-select-google-play.png#lightbox)

**Google Play API 계정** 대화 상자에서 Google Play 개발자 계정에 API 액세스를 제공하는 _클라이언트 ID_와 _클라이언트 암호_를 입력해야 합니다.

[![Google Play API 계정 대화 상자](images/xs/02-google-play-api-account-sml.png)](images/xs/02-google-play-api-account.png#lightbox)

다음 섹션에서는 새 Google API 프로젝트를 만들고 필요한 _클라이언트 ID_ 및 _클라이언트 암호_를 생성하는 방법에 대해 설명합니다.

-----


#### <a name="create-a-google-api-project"></a>Google API 프로젝트 만들기

먼저 [Google Play 개발자 계정](https://play.google.com/apps/publish)에 로그인합니다.
Google Play 개발자 계정이 없는 경우 [게시 시작](https://developer.android.com/distribute/googleplay/start.html)을 참조하세요.
Google Play 개발자 API [시작](https://developers.google.com/android-publisher/getting_started)에서도 Google Play 개발자 API 사용 방법에 대해 설명합니다. Google Play 개발자 콘솔에 로그인한 후 **설정**을 클릭합니다.

[![설정 아이콘](images/01-google-play-developer-console-sml.png)](images/01-google-play-developer-console.png#lightbox)

**설정** 페이지에서 **API 액세스**를 선택하고 **새 프로젝트 만들기** 단추를 클릭합니다.

[![새 프로젝트 만들기 단추](images/02-create-new-project-sml.png)](images/02-create-new-project.png#lightbox)

약 1분 후 새 API 프로젝트가 자동으로 생성되고 Google Play 개발자 콘솔 계정에 연결됩니다.

다음 단계는 앱에 대한 OAuth 클라이언트를 만드는 것입니다(아직 만들지 않은 경우). 사용자가 앱을 사용하여 자신의 개인 데이터에 대한 액세스를 요청하면 OAuth 클라이언트 ID를 사용하여 앱을 인증합니다.
**OAuth 클라이언트 만들기**를 클릭하여 새 OAuth 클라이언트를 만듭니다.

[![OAuth 클라이언트 만들기 단추](images/03-create-oauth-client-sml.png)](images/03-create-oauth-client.png#lightbox)

몇 초 후 새 클라이언트 ID가 생성됩니다. **Google 개발자 콘솔에서 보기**를 클릭하여 새 클라이언드 ID를 Google 개발자 콘솔에서 확인합니다.

[![표시되는 클라이언트 ID](images/04-generated-client-id-sml.png)](images/04-generated-client-id.png#lightbox)

클라이언트 ID가 이름과 만든 날짜와 함께 표시됩니다. **OAuth 클라이언트 편집** 아이콘을 클릭하여 앱의 클라이언트 암호를 확인합니다.

[![앱 자격 증명 보기](images/05-google-developer-console-sml.png)](images/05-google-developer-console.png#lightbox)

OAuth 클라이언트의 기본 이름은 *Google Play Android Developer*입니다. Xamarin.Android 앱의 이름이나 적합한 다른 이름으로 변경할 수 있습니다. 이 예제에서는 OAuth 클라이언트 이름이 앱 이름 **MyApp**으로 변경됩니다.

[![표시된 클라이언트 ID와 암호](images/06-client-id-and-secret-sml.png)](images/06-client-id-and-secret.png#lightbox)

**저장**을 클릭하여 변경 내용을 저장합니다. 그러면 **JSON 다운로드** 아이콘을 클릭하여 자격 증명을 다운로드할 수 있는**자격 증명** 페이지로 돌아갑니다. 

[![JSON 다운로드 아이콘](images/07-download-json-sml.png)](images/07-download-json.png#lightbox)

JSON 파일에는 잘라서 다음 단계의 **서명 및 배포** 대화 상자에 붙여 넣을 수 있는 클라이언트 ID과 클라이언트 암호가 있습니다.


#### <a name="register-google-api-access"></a>Google API 액세스 등록

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

클라이언트 ID와 클라이언트 암호를 사용하여 Visual Studio for Mac의 **Google Play API 계정** 대화 상자를 입력합니다. 계정에 설명을 제공할 수 있습니다. &ndash; 이렇게 하면 둘 이상의 Google Play 계정을 등록하고 향후의 APK를 다른 Google Play 계정에 등록할 수 있습니다. 클라이언트 ID와 클라이언트 암호를 이 대화 상자에 복사하고 **등록**을 클릭합니다.

[![Google API 액세스 등록 대화 상자](images/vs/05-enter-client-id-and-secret-sml.png)](images/vs/05-enter-client-id-and-secret.png#lightbox)

웹 브라우저가 열리고 Google Play Android 개발자 계정에 로그인하라는 메시지가 표시됩니다(아직 로그인하지 않은 경우). 로그인 후 웹 브라우저에 다음 메시지가 표시됩니다.
**허용**을 클릭하여 앱에 권한을 부여합니다.

[![앱 권한 부여 대화 상자](images/vs/06-authorize-app-sml.png)](images/vs/06-authorize-app.png#lightbox)

#### <a name="publish"></a>게시

**허용**을 클릭하면 브라우저가 _확인 코드 받음. 닫는 중..._  메시지가 표시되며 앱이 Visual Studio의 Google Play 계정 목록에 추가됩니다. **Google Play 계정** 대화 상자에서 **계속**을 클릭합니다.

[![Google Play 계정에 추가된 계정](images/vs/07-account-added-sml.png)](images/vs/07-account-added.png#lightbox)

다음으로 **Google Play 트랙** 대화 상자가 표시됩니다. Google Play에서는 앱 업로드를 위해 가능한 4개 트랙을 제공합니다.

* **알파** &ndash; 소규모 테스터 목록에 앱의 아주 초기 버전을 업로드하는 데 사용합니다.
* **베타** &ndash; 더 큰 규모의 테스터 목록에 앱의 초기 버전을 업로드하는 데 사용합니다.
* **출시** &ndash; 일정 비율의 사용자가 앱의 업데이트 버전을 받을 수 있습니다. 이를 통해 버그를 해결하는 동안 예를 들어 10%에서 출발하여 천천히 비율을 높여가고 100%까지 증대할 수 있습니다.
* **프로덕션** &ndash; 앱이 Google Play 스토어에서 전체 배포할 준비가 되면 이 트랙을 선택합니다.

앱 업로드에 사용할 Google Play 트랙을 선택하고 **업로드**를 클릭합니다. **출시**를 선택할 때는 백분율 값을 입력해야 합니다.

[![알파, 베타, 출시 또는 프로덕션 선택](images/vs/08-google-play-track-sml.png)](images/vs/08-google-play-track.png#lightbox)

Google Play 테스트 및 단계별 출시에 대한 자세한 내용은 [알파/베타 테스트 설정](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en)을 참조하세요.

다음으로 서명 인증서의 암호 입력을 위한 대화 상자가 표시됩니다.
암호를 입력하고 **확인**을 클릭합니다.

[![서명 암호 대화 상자](images/vs/09-certificate-password-sml.png)](images/vs/09-certificate-password.png#lightbox)

다음으로 **Archive Manager**가 업로드 진행률을 표시합니다.

[![APK 업로드 진행률](images/vs/10-uploading-apk-sml.png)](images/vs/10-uploading-apk.png#lightbox)

업로드가 완료되면 완료 상태가 Visual Studio의 왼쪽 아래에 표시됩니다.

[![프로젝트 게시 완료 메시지](images/vs/11-published-sml.png)](images/vs/11-published.png#lightbox)


### <a name="troubleshooting"></a>문제 해결

**Google Play에 게시**가 작동하려면 먼저 APK가 Google Play 스토어에 제출되어야 합니다. APK가 아직 업로드되지 않았으면 게시 마법사의 **오류** 창에 다음 오류 메시지가 표시됩니다.

[![이 앱에 대한 첫 번째 APK를 수동으로 업로드해야 합니다.](images/vs/12-upload-error-sml.png)](images/vs/12-upload-error.png#lightbox)

이 오류가 발생하면 Google Play 개발자 콘솔을 통해 APK(예: 임시 빌드)를 수동으로 업로드하고 이후의 APK 업데이트에 **배포 채널** 대화 상자를 사용합니다.  자세한 내용은 [수동으로 APK 업로드](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md)를 참조하세요. APK의 버전 코드는 업로드할 때마다 변경되어야 하며 그렇지 않으면 다음 오류가 발생합니다.

[![버전 코드 (1)가 포함된 APK가 이미 업데이트되었습니다.](images/vs/13-version-code-error-sml.png)](images/vs/13-version-code-error.png#lightbox)

이 오류를 해결하려면 다른 버전 번호로 앱을 다시 빌드하고 **배포 채널** 대화 상자를 통해 Google Play에 다시 제출합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

클라이언트 ID와 클라이언트 암호를 사용하여 Visual Studio for Mac의 **Google Play API 계정** 대화 상자를 입력합니다. 계정에 설명을 제공할 수 있습니다. &ndash; 이렇게 하면 둘 이상의 Google Play 계정을 등록하고 향후의 APK를 다른 Google Play 계정에 등록할 수 있습니다. 클라이언트 ID와 클라이언트 암호를 이 대화 상자에 복사하고 **등록**을 클릭합니다.

[![액세스 권한 부여 대화 상자](images/xs/10-register-sml.png)](images/xs/10-register.png#lightbox)

클라이언트 ID와 클라이언트 암호가 수락되면 **등록 성공** 메시지가 표시됩니다. **다음**을 클릭합니다.

[![등록 성공 메시지](images/xs/11-registration-successful-sml.png)](images/xs/11-registration-successful.png#lightbox)

**Google Play 계정** 대화 상자에서 애플리케이션 업로드를 위한 Google 계정과 트랙을 선택합니다.

[![Google 계정 선택 대화 상자](images/xs/12-choose-google-account-sml.png)](images/xs/12-choose-google-account.png#lightbox)

Google Play에서는 앱 업로드를 위해 가능한 4개 트랙을 제공합니다.

-   **알파** &ndash; 소규모 테스터 목록에 앱의 아주 초기 버전을 업로드하는 데 사용합니다.

-   **베타** &ndash; 더 큰 규모의 테스터 목록에 앱의 초기 버전을 업로드하는 데 사용합니다.

-   **출시** &ndash; 일정 비율의 사용자가 앱의 업데이트 버전을 받을 수 있습니다. 이를 통해 버그를 해결하는 동안 예를 들어 10%에서 출발하여 천천히 비율을 높여가고 100%까지 증대할 수 있습니다.

-   **프로덕션** &ndash; 앱이 Google Play 스토어에서 전체 배포할 준비가 되면 이 트랙을 선택합니다.

Google Play 테스트 및 단계별 출시에 대한 자세한 내용은 [알파/베타 테스트 설정](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en)을 참조하세요.

이제 앱에 서명하는 서명 ID를 선택합니다.
기존 서명 ID를 사용하려면 **기존 키 사용**을 선택합니다. 그렇지 않으면 [새 인증서 만들기](~/android/deploy-test/signing/index.md#newcert) 가이드에 새 키를 만드는 방법을 참조합니다. 애플리케이션에 서명할 인증서를 선택한 후 **다음**을 클릭합니다.

[![Android 서명 ID 대화 상자](images/xs/13-android-signing-identity-sml.png)](images/xs/13-android-signing-identity.png#lightbox)

이 시점에서 앱을 Google Play에 업로드할 수 있습니다. **Google Play에 게시** 대화 상자는 앱에 대한 정보를 요약합니다. &ndash; **게시**를 클릭하면 앱이 Google Play에 게시됩니다.

[![Google Play에 게시 대화 상자](images/xs/14-publish-to-google-play-sml.png)](images/xs/14-publish-to-google-play.png#lightbox)

**Google Play에 게시**가 작동하려면 먼저 APK가 Google Play 스토어에 제출되어야 합니다. APK가 업로드되지 않으면 다음 오류가 발생할 수 있습니다.

> _Google Play에서는 이 앱의 첫 번째 APK를 수동으로 업로드해야 합니다.  이 앱의 임시 APK를 사용할 수 있습니다._

또는

> _지정된 패키지 이름에 대한 애플리케이션이 없습니다. [404]_

이 오류를 해결하려면 Google Play 개발자 콘솔을 통해 APK(예: 임시 빌드)를 수동으로 업로드하고 이후의 APK 업데이트에 **Google Play에 게시** 대화 상자를 사용합니다. APK를 수동으로 업로드하는 방법에 관한 정보는 [수동으로 APK 업로드](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md)를 참조하세요.

-----
