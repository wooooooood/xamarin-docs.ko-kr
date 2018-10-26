---
title: TextureView
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2017
ms.openlocfilehash: f43147d98599d36aa136e4ddc2975205e0e69e26
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122844"
---
# <a name="textureview"></a>TextureView

`TextureView` 클래스는 비디오 또는 OpenGL 콘텐츠 스트림을 표시할 수 있도록 2D 렌더링이 하드웨어 가속을 사용 하는 뷰입니다. 예를 들어 다음 스크린샷에서 `TextureView` 장치의 카메라에서 라이브 피드를 표시 합니다.

[![장치 카메라에서 라이브 이미지의 스크린샷 예제](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

달리는 `SurfaceView` OpenGL 또는 비디오 콘텐츠를 표시 하려면 사용할 수도 있습니다, 하는 클래스를 별도 창에는 TextureView 렌더링 되지 않습니다.
따라서 `TextureView` 다른 뷰와 마찬가지로 보기 변환을 지원할 수 있습니다. 예를 들어, 회전을 `TextureView` 설정 하 여 수행할 수 있습니다 해당 `Rotation` 속성을 설정 하 여 투명도 해당 `Alpha` 속성과 등입니다.

그러므로 `TextureView` 이제 카메라에서 라이브 스트림을 표시 등을 수행 하 고 다음 코드 에서처럼 변환할 수 있습니다.

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

위의 코드를 만듭니다는 `TextureView` 활동의 인스턴스 `OnCreate` 메서드도 표현 된 활동을 가져오거나 설정 합니다 `TextureView`의 `SurfaceTextureListener`합니다. 되도록 합니다 `SurfaceTextureListener`, 활동 구현를 `TextureView.ISurfaceTextureListener` 인터페이스입니다. 시스템은 호출 된 `OnSurfaceTextAvailable` 메서드 때는 `SurfaceTexture` 사용할 준비가 되었습니다. 이 메서드에서 수행는 `SurfaceTexture` 전달을 카메라의 미리 보기 질감으로 설정 합니다. 설정 하는 등 정상적인 보기 기반 작업을 수행 하는 것은 `Rotation` 및 `Alpha`위의 예제와 같이 합니다. 응용 프로그램, 장치에서 실행 되는 다음과 같습니다.

[![이미지를 표시 하는 장치에서 실행 되는 앱의 예](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

사용 하는 `TextureView`, 하드웨어 가속을 사용 해야 API 수준 14 일부 터는 기본적으로는 것입니다. 또한이 예제에서 카메라를 사용 하므로, 모두를 `android.permission.CAMERA` 권한 및 `android.hardware.camera` 기능을 설정 해야 합니다 **AndroidManifest.xml**.



## <a name="related-links"></a>관련 링크

- [TextureViewDemo (샘플)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [Ice Cream Sandwich 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
