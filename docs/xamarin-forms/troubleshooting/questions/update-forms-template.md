---
제목: " Xamarin.Forms 기본 템플릿을 최신 NuGet 패키지로 업데이트할 수 있습니까?"
ms. 항목: 문제 해결: assetid: 160FBE13-4B4F-9248-A5CBE58FDD7F: davidbritch: dabritch:: ms. 날짜: 04/25/2017 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-package"></a>Xamarin.Forms기본 템플릿을 최신 NuGet 패키지로 업데이트할 수 있나요?

이 가이드에서는 Xamarin.Forms .NET Standard 라이브러리 템플릿을 예로 사용 하지만 동일한 일반 메서드는 Xamarin.Forms 공유 프로젝트 템플릿에도 적용 됩니다. 이 가이드는 1.5.1.6471에서 2.1.0.6529로 업데이트 하는 예제를 사용 하 여 작성 Xamarin.Forms 되었지만 동일한 단계를 통해 다른 버전을 기본값으로 설정할 수 있습니다.

1. 원본 템플릿 복사 `.zip` 위치:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2. `.zip`임시 위치에 압축을 풉니다.

3. 이전 버전의 패키지의 모든 항목 Xamarin.Forms 을 사용 하려는 새 버전으로 변경 합니다.
    * `FormsTemplate\FormsTemplate.vstemplate`
    * `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    * `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    예제: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4. 기본 [다중 프로젝트 템플릿 파일](https://msdn.microsoft.com/library/ms185308.aspx) ()의 "name" 요소를 변경 `Xamarin.Forms.PCL.vstemplate` 하 여 고유 하 게 만듭니다. 예를 들면 다음과 같습니다.

    > `<Name>Blank App (Xamarin.Forms Portable) - 2.1.0.6529</Name>`

5. 전체 템플릿 폴더의 압축을 다시 합니다. 파일의 원래 파일 구조와 일치 하는지 확인 `.zip` 합니다. `Xamarin.Forms.PCL.vstemplate`파일은 `.zip` 폴더 내에 있지 않고 파일의 맨 위에 있어야 합니다.

6. 사용자 단위 Visual Studio 템플릿 폴더에 "Mobile Apps" 하위 디렉터리를 만듭니다.
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7. 새 "Mobile Apps" 디렉터리로 새 압축 된 템플릿 폴더를 복사 합니다.

8. 3 단계의 버전과 일치 하는 NuGet 패키지를 다운로드 합니다. 예를 들어 [ https://nuget.org/api/v2/package/ Xamarin.Forms /2.1.0.6529](https://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (참고 항목 [https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file) )을 참조 하 고 Xamarin Visual Studio extensions 폴더의 해당 하위 폴더에 복사 합니다.
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`
