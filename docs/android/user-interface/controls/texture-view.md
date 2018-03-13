---
title: TextureView
ms.topic: article
ms.prod: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2017
ms.openlocfilehash: d2d9c455f2ddd652a76177527586673901edd012
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="textureview"></a>TextureView

`TextureView` 클래스는 2D 하드웨어 가속 렌더링을 사용 하 여 비디오 또는 OpenGL 콘텐츠 스트림을 표시할 수 있도록 하는 뷰입니다. 예를 들어 다음 스크린샷은 `TextureView` 장치의 카메라의 라이브 피드를 표시 합니다.

[![장치 카메라에서 라이브 이미지의 예제 스크린 샷](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

와 달리는 `SurfaceView` OpenGL 또는 비디오 콘텐츠 표시를 사용할 수도 있습니다를 클래스는 TextureView 별도 창으로 렌더링 되지 않습니다.
따라서 `TextureView` 다른 뷰와 마찬가지로 보기 변환을 지원할 수 있습니다. 예를 들어 회전는 `TextureView` 간단히 설정 하 여 수행할 수 있습니다 해당 `Rotation` 속성을 설정 하 여 투명도 해당 `Alpha` 속성 및 기타 등등.

그러므로 `TextureView` 수행할 표시 처럼 라이브 스트림을 카메라에서를 다음 코드와 같이 변형 하 고 이제 수 있습니다.

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

위의 코드 만듭니다는 `TextureView` 에서 활동의 인스턴스 `OnCreate` 메서드 활동으로 설정 하 고는 `TextureView`의 `SurfaceTextureListener`합니다. 되도록는 `SurfaceTextureListener`, 활동 구현에서 `TextureView.ISurfaceTextureListener` 인터페이스입니다. 시스템은 호출 된 `OnSurfaceTextAvailable` 메서드 때는 `SurfaceTexture` 사용할 준비가 되었습니다. 이 메서드에서 수행는 `SurfaceTexture` 에 전달 되 고 카메라의 미리 보기 질감으로 설정 합니다. 무료 설정과 같이 표준 보기 기반 작업을 수행 하는 다음의 `Rotation` 및 `Alpha`위의 예제와 같이, 합니다. 장치에서 실행 되는 결과 응용 프로그램은 다음과 같습니다.

[![이미지를 표시 하는 장치에서 실행 중인 앱의 예](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

사용 하 여 `TextureView`, 하드웨어 가속을 사용 해야 API 수준 14를 기준으로 기본적으로 됩니다. 또한 이므로 카메라를 사용 하 여이 예제 모두에서 `android.permission.CAMERA` 권한 및 `android.hardware.camera` 기능을 설정 해야 합니다는 **AndroidManifest.xml**합니다.



## <a name="related-links"></a>관련 링크

- [TextureViewDemo (샘플)](https://developer.xamarin.com/samples/monodroid/TextureViewDemo/)
- [아이스크림 샌드위치 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
