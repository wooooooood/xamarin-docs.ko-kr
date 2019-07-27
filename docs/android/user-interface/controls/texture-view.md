---
title: Xamarin Android TextureView
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2017
ms.openlocfilehash: 589cf1787f5dc3090cbfb1165e91d8ef58df37a6
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510160"
---
# <a name="xamarinandroid-textureview"></a>Xamarin Android TextureView

클래스 `TextureView` 는 비디오 또는 OpenGL 콘텐츠 스트림을 표시 하기 위해 하드웨어 가속 2d 렌더링을 사용 하는 뷰입니다. 예를 들어 다음 스크린샷은 장치 카메라에서 `TextureView` 라이브 피드를 표시 하는 방법을 보여 줍니다.

[![장치 카메라의 라이브 이미지에 대 한 예제 스크린샷](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

OpenGL 또는 비디오 콘텐츠를 표시 하는 데에도 사용할 수 있는 클래스와달리TextureView은별도의창으로렌더링되지않습니다.`SurfaceView`
`TextureView` 따라서는 다른 보기와 마찬가지로 뷰 변환을 지원할 수 있습니다. 예를 들어, 속성 `TextureView` 을 설정 하 여 `Alpha` 해당 `Rotation` 속성을 설정 하 고 투명도를 설정 하 여를 순환 시킬 수 있습니다.

따라서 이제 다음 코드 `TextureView` 와 같이 카메라에서 라이브 스트림을 표시 하 고 변환 하는 등의 작업을 수행할 수 있습니다.

```csharp
public class TextureViewActivity : Activity,
    TextureView.ISurfaceTextureListener
{
    Camera _camera;
    TextureView _textureView;
       
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        _textureView = new TextureView (this);
        _textureView.SurfaceTextureListener = this;
           
        SetContentView (_textureView);
    }
       
    public void OnSurfaceTextureAvailable (
        Android.Graphics.SurfaceTexture surface,
        int width, int height)
    {
        _camera = Camera.Open ();
        var previewSize = _camera.GetParameters ().PreviewSize;
        _textureView.LayoutParameters =
            new FrameLayout.LayoutParams (previewSize.Width,
                previewSize.Height, (int)GravityFlags.Center);

        try {
            _camera.SetPreviewTexture (surface);
            _camera.StartPreview ();
        } catch (Java.IO.IOException ex) {
            Console.WriteLine (ex.Message);
        }
           
        // this is the sort of thing TextureView enables
        _textureView.Rotation = 45.0f;
        _textureView.Alpha = 0.5f;
    }
    …
}
```

위의 코드는 `OnCreate` 활동의 `TextureView` 메서드에서 인스턴스를 만들고 활동을 `TextureView`의 `SurfaceTextureListener`로 설정 합니다. 인 경우 활동은 인터페이스를 `TextureView.ISurfaceTextureListener` 구현 합니다. `SurfaceTextureListener` `OnSurfaceTextAvailable` 를`SurfaceTexture` 사용할 준비가 되 면 시스템에서 메서드를 호출 합니다. 이 방법에서는 전달 된를 가져와 `SurfaceTexture` 카메라의 미리 보기 질감으로 설정 합니다. 그런 다음 위의 예제와 같이 `Rotation` 및 `Alpha`설정과 같은 일반적인 보기 기반 작업을 자유롭게 수행할 수 있습니다. 장치에서 실행 되는 결과 응용 프로그램은 다음과 같습니다.

[![장치에서 실행 되는 앱의 예, 이미지 표시](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

을 사용 `TextureView`하려면 하드웨어 가속을 사용 하도록 설정 해야 합니다 .이 기능은 기본적으로 API 수준 14로 설정 됩니다. 또한이 예제에서는 카메라를 사용 하기 때문에 `android.permission.CAMERA` 권한과 `android.hardware.camera` 기능 모두 **androidmanifest**에서 설정 해야 합니다.



## <a name="related-links"></a>관련 링크

- [TextureViewDemo (샘플)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [아이스크림 및 사우스 샌드위치](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](https://developer.android.com/sdk/android-4.0.html)
