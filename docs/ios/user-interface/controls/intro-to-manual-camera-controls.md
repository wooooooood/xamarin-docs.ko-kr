---
title: Xamarin.ios의 수동 카메라 컨트롤
description: 이 문서에서는 iOS AVFoundation 프레임 워크를 Xamarin.ios와 함께 사용 하 여 수동 카메라 컨트롤을 사용 하는 방법을 설명 합니다. 수동 카메라 컨트롤을 사용 하면 사용자가 포커스, 흰색 잔액 및 노출 설정을 제어할 수 있습니다.
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 6f60b52d4fd29aacf319f9de94051e28c9876e33
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226705"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Xamarin.ios의 수동 카메라 컨트롤

Ios 8 `AVFoundation Framework` 의에서 제공 하는 수동 카메라 컨트롤을 사용 하면 모바일 응용 프로그램이 ios 장치의 카메라를 완전히 제어할 수 있습니다. 이 세분화 된 제어 수준을 사용 하면 스틸 이미지 또는 비디오를 가져오는 동안 카메라의 매개 변수를 조정 하 여 전문가 수준의 카메라 응용 프로그램을 만들고 음악가 컴퍼지션을 제공할 수 있습니다.

이러한 컨트롤은 과학 또는 산업용 응용 프로그램을 개발 하는 경우에도 유용할 수 있습니다 .이 경우 결과는 이미지의 정확성 또는 장점에 맞게 조정 되지 않으며, 사용 되는 이미지의 일부 기능이 나 요소를 강조 표시 하는 데 더 적합 합니다.

## <a name="avfoundation-capture-objects"></a>AVFoundation 캡처 개체

IOS 장치에서 카메라를 사용 하 여 비디오 또는 이미지를 사용 하는 경우에도 해당 이미지를 캡처하는 데 사용 되는 프로세스는 거의 동일 합니다. 이는 기본 자동화 된 카메라 컨트롤이 나 새로운 수동 카메라 컨트롤을 활용 하는 응용 프로그램에 적용 됩니다.

 [![](intro-to-manual-camera-controls-images/image1.png "AVFoundation 캡처 개체 개요")](intro-to-manual-camera-controls-images/image1.png#lightbox)

를 사용 `AVCaptureDeviceInput` `AVCaptureSession` 하 여 `AVCaptureConnection`에서로 입력을 가져옵니다. 결과는 스틸 이미지 또는 비디오 스트림으로 출력 됩니다. 전체 프로세스는에 의해 `AVCaptureDevice`제어 됩니다.

## <a name="manual-controls-provided"></a>수동 컨트롤 제공

응용 프로그램은 iOS 8에서 제공 되는 새 Api를 사용 하 여 다음과 같은 카메라 기능을 제어할 수 있습니다.

- **수동 포커스** – 최종 사용자가 포커스를 직접 제어할 수 있도록 하 여 응용 프로그램에서 사용 되는 이미지를 보다 세밀 하 게 제어할 수 있습니다.
- **수동 노출** -노출을 수동으로 제어 하 여 응용 프로그램은 사용자에 게 보다 자유롭게 사용자를 제공 하 고 스타일을 지정할 수 있습니다.
- **수동 흰색 밸런스** – 흰색 밸런스는 이미지의 색을 조정 하는 데 사용 됩니다 .이 경우에는 자주 사용 됩니다. 광원 마다 색 온도가 다르고 이미지를 캡처하는 데 사용 되는 카메라 설정이 이러한 차이를 보정 하도록 조정 됩니다. 사용자가 사용자 정의 컨트롤을 사용 하면 사용자가 자동으로 수행할 수 없는 조정을 수행할 수 있습니다.


iOS 8은 기존 iOS Api에 대 한 확장 및 향상 된 기능을 제공 하 여 이미지 캡처 프로세스에 대 한 세분화 된 제어를 제공 합니다.

## <a name="bracketed-capture"></a>대괄호로 묶인 캡처

대괄호로 묶인 캡처는 위에 표시 된 수동 카메라 컨트롤의 설정을 기반으로 하며 응용 프로그램에서 다양 한 방식으로 시간을 캡처할 수 있도록 합니다.

간단히 말해서, 대괄호로 묶인 캡처는 그림에서 그림에 이르는 다양 한 설정을 사용 하 여 이미지를 버스트 하는 데 사용 됩니다.

## <a name="requirements"></a>요구 사항

이 문서에 제공 된 단계를 완료 하려면 다음이 필요 합니다.

- **Xcode 7 이상 및 ios 8 이상** – Apple의 Xcode 7 및 ios 8 이상 api를 개발자의 컴퓨터에 설치 하 고 구성 해야 합니다.
- **Mac용 Visual Studio** – 최신 버전의 Mac용 Visual Studio를 설치 하 고 사용자 장치에 구성 해야 합니다.
- **ios 8 장치** – 최신 버전의 ios 8을 실행 하는 ios 장치입니다. IOS 시뮬레이터에서는 카메라 기능을 테스트할 수 없습니다.


## <a name="general-av-capture-setup"></a>일반 AV 캡처 설정

IOS 장치에 비디오를 기록 하는 경우 항상 필요한 몇 가지 일반적인 설정 코드가 있습니다. 이 섹션에서는 iOS 장치의 카메라에서 비디오를 기록 하 고 해당 비디오를 실시간 `UIImageView`으로 표시 하는 데 필요한 최소 설정을 다룹니다.

### <a name="output-sample-buffer-delegate"></a>출력 샘플 버퍼 대리자

필요한 첫 번째 작업 중 하나는 샘플 출력 버퍼를 모니터링 하 고 응용 프로그램 UI `UIImageView` 의 버퍼에서로 이미지 grabbed 표시 하는 대리자입니다.

다음 루틴은 샘플 버퍼를 모니터링 하 고 UI를 업데이트 합니다.

```csharp
using System;
using Foundation;
using UIKit;
using System.CodeDom.Compiler;
using System.Collections.Generic;
using System.Linq;
using AVFoundation;
using CoreVideo;
using CoreMedia;
using CoreGraphics;

namespace ManualCameraControls
{
    public class OutputRecorder : AVCaptureVideoDataOutputSampleBufferDelegate
    {
        #region Computed Properties
        public UIImageView DisplayView { get; set; }
        #endregion

        #region Constructors
        public OutputRecorder ()
        {

        }
        #endregion

        #region Private Methods
        private UIImage GetImageFromSampleBuffer(CMSampleBuffer sampleBuffer) {

            // Get a pixel buffer from the sample buffer
            using (var pixelBuffer = sampleBuffer.GetImageBuffer () as CVPixelBuffer) {
                // Lock the base address
                pixelBuffer.Lock (0);

                // Prepare to decode buffer
                var flags = CGBitmapFlags.PremultipliedFirst | CGBitmapFlags.ByteOrder32Little;

                // Decode buffer - Create a new colorspace
                using (var cs = CGColorSpace.CreateDeviceRGB ()) {

                    // Create new context from buffer
                    using (var context = new CGBitmapContext (pixelBuffer.BaseAddress,
                        pixelBuffer.Width,
                        pixelBuffer.Height,
                        8,
                        pixelBuffer.BytesPerRow,
                        cs,
                        (CGImageAlphaInfo)flags)) {

                        // Get the image from the context
                        using (var cgImage = context.ToImage ()) {

                            // Unlock and return image
                            pixelBuffer.Unlock (0);
                            return UIImage.FromImage (cgImage);
                        }
                    }
                }
            }
        }
        #endregion

        #region Override Methods
        public override void DidOutputSampleBuffer (AVCaptureOutput captureOutput, CMSampleBuffer sampleBuffer, AVCaptureConnection connection)
        {
            // Trap all errors
            try {
                // Grab an image from the buffer
                var image = GetImageFromSampleBuffer(sampleBuffer);

                // Display the image
                if (DisplayView !=null) {
                    DisplayView.BeginInvokeOnMainThread(() => {
                        // Set the image
                        if (DisplayView.Image != null) DisplayView.Image.Dispose();
                        DisplayView.Image = image;

                        // Rotate image to the correct display orientation
                        DisplayView.Transform = CGAffineTransform.MakeRotation((float)Math.PI/2);
                    });
                }

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            }
            catch(Exception e) {
                // Report error
                Console.WriteLine ("Error sampling buffer: {0}", e.Message);
            }
        }
        #endregion
    }
}
```

이 루틴을 사용 하면에서 `AppDelegate` AV 캡처 세션을 열어 라이브 비디오 피드를 기록 하도록를 수정할 수 있습니다.

### <a name="creating-an-av-capture-session"></a>AV 캡처 세션 만들기

AV 캡처 세션은 iOS 장치의 카메라에서 라이브 비디오 기록을 제어 하는 데 사용 되며 iOS 응용 프로그램에 비디오를 가져오는 데 필요 합니다. 예제 `ManualCameraControl` 샘플 응용 프로그램은 여러 위치에서 캡처 세션을 사용 하 `AppDelegate` 고 있으므로에서 구성 되 고 전체 응용 프로그램에서 사용할 수 있게 됩니다.

응용 프로그램 `AppDelegate` 을 수정 하 고 필요한 코드를 추가 하려면 다음을 수행 합니다.


1. 솔루션 탐색기 `AppDelegate.cs` 파일을 두 번 클릭 하 여 편집용으로 엽니다.
1. 다음 using 문을 파일 맨 위에 추가 합니다.

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    ```

1. `AppDelegate` 클래스에 다음 전용 변수 및 계산 된 속성을 추가 합니다.

    ```csharp
    #region Private Variables
    private NSError Error;
    #endregion

    #region Computed Properties
    public override UIWindow Window {get;set;}
    public bool CameraAvailable { get; set; }
    public AVCaptureSession Session { get; set; }
    public AVCaptureDevice CaptureDevice { get; set; }
    public OutputRecorder Recorder { get; set; }
    public DispatchQueue Queue { get; set; }
    public AVCaptureDeviceInput Input { get; set; }
    #endregion
    ```

1. 완성 된 메서드를 재정의 하 고 다음과 같이 변경 합니다.

    ```csharp
    public override void FinishedLaunching (UIApplication application)
    {
        // Create a new capture session
        Session = new AVCaptureSession ();
        Session.SessionPreset = AVCaptureSession.PresetMedium;

        // Create a device input
        CaptureDevice = AVCaptureDevice.DefaultDeviceWithMediaType (AVMediaType.Video);
        if (CaptureDevice == null) {
            // Video capture not supported, abort
            Console.WriteLine ("Video recording not supported on this device");
            CameraAvailable = false;
            return;
        }

        // Prepare device for configuration
        CaptureDevice.LockForConfiguration (out Error);
        if (Error != null) {
            // There has been an issue, abort
            Console.WriteLine ("Error: {0}", Error.LocalizedDescription);
            CaptureDevice.UnlockForConfiguration ();
            return;
        }

        // Configure stream for 15 frames per second (fps)
        CaptureDevice.ActiveVideoMinFrameDuration = new CMTime (1, 15);

        // Unlock configuration
        CaptureDevice.UnlockForConfiguration ();

        // Get input from capture device
        Input = AVCaptureDeviceInput.FromDevice (CaptureDevice);
        if (Input == null) {
            // Error, report and abort
            Console.WriteLine ("Unable to gain input from capture device.");
            CameraAvailable = false;
            return;
        }

        // Attach input to session
        Session.AddInput (Input);

        // Create a new output
        var output = new AVCaptureVideoDataOutput ();
        var settings = new AVVideoSettingsUncompressed ();
        settings.PixelFormatType = CVPixelFormatType.CV32BGRA;
        output.WeakVideoSettings = settings.Dictionary;

        // Configure and attach to the output to the session
        Queue = new DispatchQueue ("ManCamQueue");
        Recorder = new OutputRecorder ();
        output.SetSampleBufferDelegate (Recorder, Queue);
        Session.AddOutput (output);

        // Let tabs know that a camera is available
        CameraAvailable = true;
    }
    ```

1. 파일의 변경 내용을 저장합니다.


이 코드를 사용 하면 실험 및 테스트를 위해 수동 카메라 컨트롤을 쉽게 구현할 수 있습니다.

## <a name="manual-focus"></a>수동 포커스

최종 사용자가 포커스를 직접 제어할 수 있도록 하 여 응용 프로그램은 촬영 된 이미지를 보다 다양 하 게 제어할 수 있습니다.

예를 들어 전문 사진 작가는 이미지의 초점을 부드럽게 하 여 [Bokeh 효과](https://en.wikipedia.org/wiki/Bokeh)를 달성할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image2.png "Bokeh 효과")](intro-to-manual-camera-controls-images/image2.png#lightbox)

또는 다음과 같이 [포커스 풀 효과](http://www.mediacollege.com/video/camera/focus/pull.html)를 만듭니다.

[![](intro-to-manual-camera-controls-images/image3.png "포커스 풀 효과")](intro-to-manual-camera-controls-images/image3.png#lightbox)

과학자 또는 의료 응용 프로그램 작성자의 경우 응용 프로그램은 실험을 위해 렌즈를 프로그래밍 방식으로 이동 하려고 할 수 있습니다. 새 API를 사용 하면 이미지를 만들 때 최종 사용자 또는 응용 프로그램에서 포커스를 제어할 수 있습니다.

### <a name="how-focus-works"></a>포커스 작동 방식

IOS 8 응용 프로그램에서 포커스 제어의 세부 사항에 대해 설명 합니다. IOS 장치에서 포커스가 어떻게 작동 하는지 간략히 살펴보겠습니다.

[![](intro-to-manual-camera-controls-images/image4.png "IOS 장치에서 포커스가 작동 하는 방식")](intro-to-manual-camera-controls-images/image4.png#lightbox)

조명이 iOS 장치에서 카메라 렌즈로 들어오고 이미지 센서에 초점을 맞추고 있습니다. 센서 컨트롤의 렌즈 거리 (이미지가 sharpest 표시 되는 영역)는 센서와 관련 되어 있습니다. 렌즈는 센서에서 멀리 떨어져 있으며 거리 개체는 sharpest 것 처럼 보이지만 가까운 개체는 sharpest로 보입니다.

IOS 장치에서 렌즈는 가까운 자석 및 스프링에서 센서에 가깝게 (또는 그 이상) 이동 합니다. 따라서 장치에 따라 달라 지는 렌즈의 정확한 위치는 불가능 하며 장치의 방향 또는 장치 및 스프링의 사용 기간과 같은 매개 변수의 영향을 받을 수 있습니다.

### <a name="important-focus-terms"></a>중요 한 포커스 용어

포커스를 처리할 때 개발자가 알아야 할 몇 가지 용어가 있습니다.

- **필드의 깊이** – 가장 가까이와 가장 가까이 있는 개체 사이의 거리입니다.
- **매크로** -포커스 스펙트럼의 가까운 끝 이며, 렌즈가 포커스를 이동할 수 있는 가장 가까운 거리입니다.
- **Infinity** – 포커스 스펙트럼의 끝 부분으로, 렌즈가 집중할 수 있는 가장 먼 거리입니다.
- 하이퍼 **초점 거리** – 프레임의 가장 먼 개체가 포커스의 맨 끝에만 있는 포커스 스펙트럼의 지점입니다. 즉, 필드의 깊이를 최대화 하는 초점면 위치가 여기에 해당 합니다.
- **렌즈 위치** – 위의 모든 용어를 제어 하는 것입니다. 센서의 렌즈와 포커스의 컨트롤러 사이의 거리입니다.


이러한 용어와 정보를 염두에 두고 새로운 수동 포커스 컨트롤을 iOS 8 응용 프로그램에서 성공적으로 구현할 수 있습니다.

### <a name="existing-focus-controls"></a>기존 포커스 컨트롤

iOS 7 및 이전 버전에서는 기존 포커스가 다음과 같이 속성을 `FocusMode`통해 제어 됩니다.

- `AVCaptureFocusModeLocked`– 포커스는 단일 포커스 지점에서 잠깁니다.
- `AVCaptureFocusModeAutoFocus`– 카메라는 선명 하 게 포커스를 찾은 다음 해당 위치에 유지 될 때까지 모든 초점을 통해 렌즈를 스윕 합니다.
- `AVCaptureFocusModeContinuousAutoFocus`– 카메라가 포커스를 벗어난 상태를 감지할 때마다 refocuses.


또한 기존 컨트롤은 사용자가 특정 영역에 초점을 맞출`FocusPointOfInterest` 수 있도록 속성을 통해 설정 가능한 관심 지점을 제공 합니다. 응용 프로그램은 속성을 `IsAdjustingFocus` 모니터링 하 여 렌즈 움직임을 추적할 수도 있습니다.

또한 `AutoFocusRangeRestriction` 속성에서 범위 제한이 다음과 같이 제공 되었습니다.

- `AVCaptureAutoFocusRangeRestrictionNear`– 자동 포커스를 주변 깊이에 제한 합니다. QR 코드 또는 바코드를 스캔 하는 경우에 유용 합니다.
- `AVCaptureAutoFocusRangeRestrictionFar`– Autofocus를 원거리 수준으로 제한 합니다. 관련이 없는 것으로 알려진 개체가 뷰 필드 (예를 들어 창 프레임)에 있는 경우에 유용 합니다.


마지막으로 자동 포커스 `SmoothAutoFocus` 알고리즘의 속도를 높이는 속성을 사용 하 고 비디오를 녹화할 때 아티팩트 이동을 방지 하기 위해 더 작은 증분으로 단계를 수행 합니다.

### <a name="new-focus-controls-in-ios-8"></a>IOS 8의 새로운 포커스 컨트롤

IOS 7 이상에서 제공 하는 기능 외에도 다음과 같은 기능을 사용 하 여 iOS 8에서 포커스를 제어할 수 있습니다.

- 포커스를 잠글 때 렌즈 위치를 완전히 수동으로 제어 합니다.
- 포커스 모드에서 렌즈 위치의 키-값 관찰.


위의 기능을 구현 하기 위해 클래스 `AVCaptureDevice` 는 카메라 렌즈의 현재 위치를 가져오는 데 사용 `LensPosition` 되는 읽기 전용 속성을 포함 하도록 수정 되었습니다.

렌즈 위치를 수동으로 제어 하려면 캡처 장치가 잠긴 포커스 모드에 있어야 합니다. 예제:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

캡처 `SetFocusModeLocked` 장치의 메서드는 카메라 렌즈의 위치를 조정 하는 데 사용 됩니다. 변경 내용이 적용 될 때 알림을 받으려면 선택적 콜백 루틴을 제공할 수 있습니다. 예제:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

위의 코드에서 볼 수 있듯이, 캡처 장치를 구성 하려면 먼저 잠금 상태를 변경 해야 합니다. 유효한 렌즈 위치 값은 0.0에서 1.0 사이입니다.

### <a name="manual-focus-example"></a>수동 포커스 예제

일반 AV 캡처 설정 코드가 준비 되 면를 `UIViewController` 응용 프로그램의 스토리 보드에 추가 하 고 다음과 같이 구성할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image5.png "UIViewController는 응용 프로그램 스토리 보드에 추가 하 고 여기에 표시 된 대로 구성할 수 있습니다.")](intro-to-manual-camera-controls-images/image5.png#lightbox)

뷰에는 다음과 같은 주요 요소가 포함 되어 있습니다.

- 비디오 피드를 표시 하는입니다.`UIImageView`
- 포커스 모드를 자동에서 잠김으로 변경 하는입니다.`UISegmentedControl`
- 현재 렌즈 위치를 표시 하 고 업데이트 하는입니다.`UISlider`


수동 포커스 제어를 위해 보기 컨트롤러를 연결 하려면 다음을 수행 합니다.


1. 다음 using 문을 추가 합니다.

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```

1. 다음 전용 변수를 추가 합니다.

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```

1. 다음 계산 된 속성을 추가 합니다.

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. 메서드를 `ViewDidLoad` 재정의 하 고 다음 코드를 추가 합니다.

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Position.BeginInvokeOnMainThread(() =>{
                Position.Value = ThisApp.Input.Device.LensPosition;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {

            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);

            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto focus and start monitoring position
                Position.Enabled = false;
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.ContinuousAutoFocus;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Stop auto focus and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;
                Automatic = false;
                Position.Enabled = true;
                break;
            }

            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        // Monitor position changes
        Position.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic) return;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```

1. 메서드를 `ViewDidAppear` 재정의 하 고 다음을 추가 하 여 뷰가 로드 될 때 기록을 시작 합니다.

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```

1. 카메라를 Auto 모드에서 사용 하면 카메라가 포커스를 조정할 때 슬라이더가 자동으로 이동 됩니다.

    [![](intro-to-manual-camera-controls-images/image6.png "카메라가이 샘플 앱에서 포커스를 조정할 때 슬라이더가 자동으로 이동 됩니다.")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. 잠긴 세그먼트를 탭 하 고 위치 슬라이더를 끌어서 렌즈 위치를 수동으로 조정 합니다.

    [![](intro-to-manual-camera-controls-images/image7.png "수동으로 렌즈 위치 조정")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. 응용 프로그램을 중지 합니다.


위의 코드는 카메라가 자동 모드에 있을 때 렌즈 위치를 모니터링 하거나 슬라이더를 사용 하 여 잠긴 모드에 있을 때 렌즈 위치를 제어 하는 방법을 보여 줍니다.

## <a name="manual-exposure"></a>수동 노출

노출은 원본 밝기를 기준으로 하는 이미지의 밝기를 나타내며 센서의 적중 횟수, 시간 및 센서의 게인 수준 (ISO 매핑)에 따라 결정 됩니다. 응용 프로그램은 노출에 대 한 수동 제어를 제공 하 여 최종 사용자에 게 보다 자유롭게 제공 하 고 스타일을 지정할 수 있습니다.

사용자는 수동 노출 컨트롤을 사용 하 여 통해 비현실적으로에서 어두운 및 moody로 이미지를 가져올 수 있습니다.

[![](intro-to-manual-camera-controls-images/image8.png "통해 비현실적으로 밝은에서 어두운 및 moody의 노출을 보여주는 이미지 샘플")](intro-to-manual-camera-controls-images/image8.png#lightbox)

이 작업은 과학 응용 프로그램의 프로그래밍 방식 제어를 사용 하거나 응용 프로그램 사용자 인터페이스에서 제공 하는 수동 컨트롤을 통해 자동으로 수행할 수 있습니다. 어느 쪽이 든 새 iOS 8 노출 Api는 카메라의 노출 설정에 대해 세분화 된 제어를 제공 합니다.

### <a name="how-exposure-works"></a>노출의 작동 방식

IOS 8 응용 프로그램에서 노출을 제어 하는 방법에 대 한 자세한 내용은을 참조 하세요. 노출이 어떻게 작동 하는지 간략히 살펴보겠습니다.

[![](intro-to-manual-camera-controls-images/image9.png "노출의 작동 방식")](intro-to-manual-camera-controls-images/image9.png#lightbox)

노출을 제어 하기 위해 함께 제공 되는 세 가지 기본 요소는 다음과 같습니다.

- **셔터 속도** – 카메라 센서를 밝게 하기 위해 셔터를 여는 데 걸리는 시간입니다. 셔터를 여는 시간이 짧을수록 빛이 줄어들고 이미지가 더 선명해 지는 것이 좋습니다 (동작 흐림 효과 낮음). 셔터를 더 길게 열면에서 더 많은 조명을 사용 하 고 더 많은 동작 흐림을 발생 합니다.
- **ISO 매핑** – 필름 사진에서 빌려 온 용어 이며 필름의 화학 물질의 민감도를 나타냅니다. 필름의 낮은 ISO 값은 더 적고 색을 더 세밀 하 게 복제 합니다. 디지털 센서의 낮은 ISO 값은 센서 노이즈가 적고 밝기가 낮습니다. ISO 값이 높을수록 이미지의 밝기는 높아지지만 센서 소음은 더 커집니다. 디지털 센서의 "ISO"는 물리적 기능이 아니라 [전자적 이득을](https://en.wikipedia.org/wiki/Gain)측정 한 것입니다.
- **렌즈 애퍼처** – 렌즈를 여는 크기입니다. 모든 iOS 장치에서 렌즈 애퍼처는 고정 되어 있으므로 노출을 조정 하는 데 사용할 수 있는 두 값은 셔터 속도와 ISO입니다.


### <a name="how-continuous-auto-exposure-works"></a>연속 자동 노출이 작동 하는 방식

수동 노출이 어떻게 작동 하는지 알아보기 전에 iOS 장치에서 지속적인 자동 노출이 작동 하는 방식을 이해 하는 것이 좋습니다.

[![](intro-to-manual-camera-controls-images/image10.png "IOS 장치에서 연속 자동 노출이 작동 하는 방식")](intro-to-manual-camera-controls-images/image10.png#lightbox)

첫 번째는 자동 노출 블록 이며 이상적인 노출을 계산 하 고 계속 해 서 측정 통계를 공급 하는 작업을 포함 합니다. 이 정보를 사용 하 여 가장 적합 한 ISO 및 셔터 속도 혼합을 계산 하 여 장면을 잘 켜 집니다. 이 주기를 AE 루프 라고 합니다.

### <a name="how-locked-exposure-works"></a>잠긴 노출의 작동 방식

다음으로, iOS 장치에서 잠긴 노출이 어떻게 작동 하는지 살펴보겠습니다.

[![](intro-to-manual-camera-controls-images/image11.png "IOS 장치에서 잠긴 노출이 작동 하는 방식")](intro-to-manual-camera-controls-images/image11.png#lightbox)

또한 최적의 iOS 및 기간 값을 계산 하는 자동 노출 블록이 있습니다. 그러나이 모드에서 AE 블록은 계량 통계 엔진에서 연결이 끊어집니다.

### <a name="existing-exposure-controls"></a>기존 노출 컨트롤

iOS 7 이상에서는 `ExposureMode` 속성을 통해 다음과 같은 기존 노출 컨트롤을 제공 합니다.

- `AVCaptureExposureModeLocked`– 장면을 한 번 샘플링 하 고 장면 전체에서 해당 값을 사용 합니다.
- `AVCaptureExposureModeContinuousAutoExposure`– 장면을 연속 해 서 샘플링 하 여 제대로 작동 하는지 확인 합니다.


을 (를) 사용 하 여 노출할 대상 개체를 선택 하 여 장면을 노출 하 고, 응용 프로그램에서 속성을 `AdjustingExposure` 모니터링 하 여 노출이 조정 될 때 볼 수 있습니다. `ExposurePointOfInterest`

### <a name="new-exposure-controls-in-ios-8"></a>IOS 8의 새로운 노출 컨트롤

IOS 7 이상에서 제공 하는 기능 외에도 다음과 같은 기능을 사용 하 여 iOS 8에서 노출을 제어할 수 있습니다.

- 완전히 수동 사용자 지정 노출.
- Get, Set 및 Key 값은 IOS 및 셔터 속도 (기간)를 관찰 합니다.


위의 기능을 구현 하기 위해 새 `AVCaptureExposureModeCustom` 모드가 추가 되었습니다. 의 카메라가 사용자 지정 모드 이면 다음 코드를 사용 하 여 노출 기간 및 ISO를 조정할 수 있습니다.

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

자동 및 잠김 모드에서 응용 프로그램은 다음 코드를 사용 하 여 자동 노출 루틴의 바이어스를 조정할 수 있습니다.

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

최소 및 최대 설정 범위는 응용 프로그램이 실행 되는 장치에 따라 달라 지므로 하드 코드 되어서는 안 됩니다. 대신 다음 속성을 사용 하 여 최소 및 최대 값 범위를 가져옵니다.

- `CaptureDevice.MinExposureTargetBias`
- `CaptureDevice.MaxExposureTargetBias`
- `CaptureDevice.ActiveFormat.MinISO`
- `CaptureDevice.ActiveFormat.MaxISO`
- `CaptureDevice.ActiveFormat.MinExposureDuration`
- `CaptureDevice.ActiveFormat.MaxExposureDuration`


위의 코드에서 볼 수 있듯이 캡처 장치를 구성 하려면 먼저 잠금 상태를 변경 해야 노출이 변경 될 수 있습니다.

### <a name="manual-exposure-example"></a>수동 노출 예

일반 AV 캡처 설정 코드가 준비 되 면를 `UIViewController` 응용 프로그램의 스토리 보드에 추가 하 고 다음과 같이 구성할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image12.png "UIViewController는 응용 프로그램 스토리 보드에 추가 하 고 여기에 표시 된 대로 구성할 수 있습니다.")](intro-to-manual-camera-controls-images/image12.png#lightbox)

뷰에는 다음과 같은 주요 요소가 포함 되어 있습니다.

- 비디오 피드를 표시 하는입니다.`UIImageView`
- 포커스 모드를 자동에서 잠김으로 변경 하는입니다.`UISegmentedControl`
- 오프셋 `UISlider` , 기간, ISO 및 바이어스를 표시 하 고 업데이트 하는 네 가지 컨트롤.


다음을 수행 하 여 수동 노출 제어를 위해 뷰 컨트롤러를 연결 합니다.


1. 다음 using 문을 추가 합니다.

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```

1. 다음 전용 변수를 추가 합니다.

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    private nfloat ExposureDurationPower = 5;
    private nfloat ExposureMinimumDuration = 1.0f/1000.0f;
    #endregion
    ```

1. 다음 계산 된 속성을 추가 합니다.

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. 메서드를 `ViewDidLoad` 재정의 하 고 다음 코드를 추가 합니다.

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Offset.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Offset.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;

        Duration.MinValue = 0.0f;
        Duration.MaxValue = 1.0f;

        ISO.MinValue = ThisApp.CaptureDevice.ActiveFormat.MinISO;
        ISO.MaxValue = ThisApp.CaptureDevice.ActiveFormat.MaxISO;

        Bias.MinValue = ThisApp.CaptureDevice.MinExposureTargetBias;
        Bias.MaxValue = ThisApp.CaptureDevice.MaxExposureTargetBias;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Update position slider
            Offset.BeginInvokeOnMainThread(() =>{
                Offset.Value = ThisApp.Input.Device.ExposureTargetOffset;
            });

            Duration.BeginInvokeOnMainThread(() =>{
                var newDurationSeconds = CMTimeGetSeconds(ThisApp.Input.Device.ExposureDuration);
                var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration), ExposureMinimumDuration);
                var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
                var p = (newDurationSeconds - minDurationSeconds) / (maxDurationSeconds - minDurationSeconds);
                Duration.Value = (float)Math.Pow(p, 1.0f/ExposureDurationPower);
            });

            ISO.BeginInvokeOnMainThread(() => {
                ISO.Value = ThisApp.Input.Device.ISO;
            });

            Bias.BeginInvokeOnMainThread(() => {
                Bias.Value = ThisApp.Input.Device.ExposureTargetBias;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (object sender, EventArgs e) => {

            // Lock device for change
            ThisApp.CaptureDevice.LockForConfiguration(out Error);

            // Take action based on the segment selected
            switch(Segments.SelectedSegment) {
            case 0:
                // Activate auto exposure and start monitoring position
                Duration.Enabled = false;
                ISO.Enabled = false;
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.ContinuousAutoExposure;
                SampleTimer.Start();
                Automatic = true;
                break;
            case 1:
                // Lock exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Locked;
                Automatic = false;
                Duration.Enabled = false;
                ISO.Enabled = false;
                break;
            case 2:
                // Custom exposure and allow the user to control the camera
                SampleTimer.Stop();
                ThisApp.CaptureDevice.ExposureMode = AVCaptureExposureMode.Custom;
                Automatic = false;
                Duration.Enabled = true;
                ISO.Enabled = true;
                break;
            }

            // Unlock device
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        // Monitor position changes
        Duration.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic) return;

            // Calculate value
            var p = Math.Pow(Duration.Value,ExposureDurationPower);
            var minDurationSeconds = Math.Max(CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MinExposureDuration),ExposureMinimumDuration);
            var maxDurationSeconds = CMTimeGetSeconds(ThisApp.CaptureDevice.ActiveFormat.MaxExposureDuration);
            var newDurationSeconds = p * (maxDurationSeconds - minDurationSeconds) +minDurationSeconds;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(CMTime.FromSeconds(p,1000*1000*1000),ThisApp.CaptureDevice.ISO,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        ISO.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic) return;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.LockExposure(ThisApp.CaptureDevice.ExposureDuration,ISO.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };

        Bias.ValueChanged += (object sender, EventArgs e) => {

            // If we are in the automatic mode, ignore changes
            // if (Automatic) return;

            // Update Focus position
            ThisApp.CaptureDevice.LockForConfiguration(out Error);
            ThisApp.CaptureDevice.SetExposureTargetBias(Bias.Value,null);
            ThisApp.CaptureDevice.UnlockForConfiguration();
        };
    }
    ```

1. 메서드를 `ViewDidAppear` 재정의 하 고 다음을 추가 하 여 뷰가 로드 될 때 기록을 시작 합니다.

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```

1. 카메라를 Auto 모드로 사용 하면 카메라에서 노출이 조정 될 때 슬라이더가 자동으로 이동 합니다.

    [![](intro-to-manual-camera-controls-images/image13.png "카메라가 노출을 조정 하면 슬라이더가 자동으로 이동 합니다.")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. 잠긴 세그먼트를 탭 하 고 편향 슬라이더를 끌어서 자동 노출의 바이어스를 수동으로 조정 합니다.

    [![](intro-to-manual-camera-controls-images/image14.png "자동 노출의 바이어스를 수동으로 조정")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. 사용자 지정 세그먼트를 탭 하 고 기간 및 ISO 슬라이더를 끌어서 노출을 수동으로 제어 합니다.

    [![](intro-to-manual-camera-controls-images/image15.png "재생 시간 및 ISO 슬라이더를 끌어서 노출을 수동으로 제어")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. 응용 프로그램을 중지 합니다.


위의 코드는 카메라가 자동 모드에 있을 때 노출 설정을 모니터링 하는 방법과 슬라이더를 사용 하 여 잠겨 있거나 사용자 지정 모드에 있을 때 노출을 제어 하는 방법을 보여 줍니다.

## <a name="manual-white-balance"></a>수동 흰색 잔액

사용자는 흰색 밸런스 컨트롤을 사용 하 여 이미지에서 colosr의 균형을 조정 하 여 더 사실적인 느낌을 만들 수 있습니다. 광원 마다 색 온도가 다르고 이미지를 캡처하는 데 사용 되는 카메라 설정을 조정 하 여 이러한 차이를 보정 해야 합니다. 또한 사용자 컨트롤을 흰색 잔액에 맞게 허용 하 여 자동 루틴이 미술 효과를 달성할 수 없도록 하는 전문 조정을 수행할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image16.png "수동 흰색 밸런스 조정을 보여 주는 샘플 이미지")](intro-to-manual-camera-controls-images/image16.png#lightbox)

예를 들어 일광에는 blueish 캐스트가 있지만 텅스텐 incandescent 광원에는 핫, 노랑-주황 색조가 있습니다. (혼동, "쿨" 색은 "웜" 색 보다 높은 색 온도를 가집니다. 색 온도는 가시 범위 1이 아닌 실제 측정값입니다.

인간 마인드는 색 온도의 차이를 보정 하는 데 매우 유용 하지만 카메라에서 수행할 수 없는 작업입니다. 카메라는 색의 차이를 조정 하기 위해 반대쪽 스펙트럼에서 색을 높이는 방식으로 작동 합니다.

새 iOS 8 노출 API를 통해 응용 프로그램은 프로세스를 제어 하 고 카메라의 흰색 잔액 설정에 대해 세분화 된 제어를 제공할 수 있습니다.

### <a name="how-white-balance-works"></a>흰색 잔액의 작동 방식

IOS 8 응용 프로그램의 흰색 잔액을 제어 하는 방법에 대 한 자세한 내용은을 참조 하세요. 잔액이 어떻게 작동 하는지 간략히 살펴보겠습니다.

색 인식 연구에서 [CIE 1931 RGB 색 공간 및 CIE 1931 XYZ 색 공간은](https://en.wikipedia.org/wiki/CIE_1931_color_space) 첫 번째 수학적으로 정의 된 색 공간입니다. 1931의 조명 (CIE)에 대 한 국제 위원회에 의해 생성 되었습니다.

[![](intro-to-manual-camera-controls-images/image17.png "CIE 1931 RGB 색 공간 및 CIE 1931 XYZ 색 공간")](intro-to-manual-camera-controls-images/image17.png#lightbox)

위의 차트에서는 진한 파란색에서 밝은 녹색에서 밝은 빨강으로의 인간 눈에 보이는 모든 색을 보여 줍니다. 위의 그래프에 표시 된 것 처럼 다이어그램의 모든 점을 X 및 Y 값으로 그릴 수 있습니다.

그래프에 표시 된 것 처럼 사용자 비전 범위 밖에 있는 그래프에 그릴 수 있는 X 및 Y 값이 있으며, 그 결과 이러한 색은 카메라에 의해 재현 되지 않습니다.

위의 차트에서 더 작은 곡선을 [Planckian Locus](https://en.wikipedia.org/wiki/Planckian_locus)라고 하며,이는 파란색 측면 (더울)에서 더 높은 숫자를 사용 하 고 빨강 측면에서 더 낮은 숫자 (냉각기)를 사용 하 여 색 온도 (켈빈)를 나타냅니다. 이러한 설정은 일반적인 조명 상황에서 유용 합니다.

혼합 된 조명 조건에서 필요한 변경을 수행 하려면 Planckian Locus를 사용 해야 합니다. 이러한 상황에서 조정은 CIE 눈금의 녹색 또는 빨간색/자홍 면으로 이동 해야 합니다.

iOS 장치는 이와 반대 되는 색 획득을 통해 색 캐스트를 보정 합니다. 예를 들어 장면의 파란색이 너무 많은 경우에는 빨강 이득을 보정 하기 위해 승격 됩니다. 이러한 획득 값은 장치에 따라 달라 지는 특정 장치에 맞게 보정 됩니다.

### <a name="existing-white-balance-controls"></a>기존 흰색 잔액 컨트롤

iOS 7 이상에서는 속성을 통해 `WhiteBalanceMode` 다음과 같은 기존 흰색 잔액 컨트롤을 제공 했습니다.

- `AVCapture WhiteBalance ModeLocked`– 장면을 한 번 샘플링 하 고 장면 전체에 해당 값을 사용 합니다.
- `AVCapture WhiteBalance ModeContinuousAutoExposure`– 적절 한 균형을 유지 하기 위해 장면을 지속적으로 샘플링 합니다.


그리고 응용 프로그램은 `AdjustingWhiteBalance` 속성을 모니터링 하 여 노출이 조정 될 때를 확인할 수 있습니다.

### <a name="new-white-balance-controls-in-ios-8"></a>IOS 8의 새로운 흰색 잔액 컨트롤

IOS 7 이상에서 제공 하는 기능 외에도 다음과 같은 기능을 사용 하 여 iOS 8의 흰색 균형을 제어할 수 있습니다.

- 장치 RGB 이득을 완전히 수동으로 제어 합니다.
- Get, Set 및 Key 값은 장치 RGB 이득을 관찰 합니다.
- 회색 카드를 사용한 흰색 밸런스 지원.
- 장치 독립적 색 공간에 대 한 변환 루틴.


위의 기능 `AVCaptureWhiteBalanceGain` 을 구현 하기 위해 구조가 다음과 같은 멤버와 함께 추가 되었습니다.

- `RedGain`
- `GreenGain`
- `BlueGain`


최대 잔액 이득은 현재 4 (4) 이며 `MaxWhiteBalanceGain` 속성에서 준비할 수 있습니다. 따라서 올바른 범위는 (1)에서 (4) `MaxWhiteBalanceGain` 로 현재입니다.

`DeviceWhiteBalanceGains` 속성을 사용 하 여 현재 값을 관찰할 수 있습니다. 카메라가 `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` 잠금 상태를 유지 하는 경우를 사용 하 여 잔액 효과를 조정 합니다.

#### <a name="conversion-routines"></a>변환 루틴

변환 루틴은 장치 독립적 색 공간으로 변환 하는 데 도움이 되도록 iOS 8에 추가 되었습니다. 변환 루틴을 `AVCaptureWhiteBalanceChromaticityValues` 구현 하기 위해 구조가 다음과 같은 멤버와 함께 추가 되었습니다.

- `X`-0에서 1 사이의 값입니다.
- `Y`-0에서 1 사이의 값입니다.


또한 다음과 같은 멤버를 사용 하 여 구조체를추가했습니다.`AVCaptureWhiteBalanceTemperatureAndTintValues`

- `Temperature`-절대 온도 단위의 부동 소수점 값입니다.
- `Tint`-0에서 150 사이의 오프셋으로, 양수 값은 녹색 방향으로, 자홍에서는 음수입니다.


`CaptureDevice.GetTemperatureAndTintValues` 및메서드를사용하여온도와색조,chromaticity및RGB게인색`CaptureDevice.GetDeviceWhiteBalanceGains`공간 간을 변환 합니다.

> [!NOTE]
> 변환 루틴은 변환할 값이 Planckian Locus에 가까울수록 더 정확 합니다.




#### <a name="gray-card-support"></a>회색 카드 지원

Apple은 회색 세계 용어를 사용 하 여 iOS 8에 기본 제공 되는 회색 카드 지원을 나타냅니다. 이를 통해 사용자는 프레임 중앙의 50% 이상을 포함 하는 실제 회색 카드에 초점을 맞춘 후이를 사용 하 여 균형을 조정할 수 있습니다. 회색 카드의 목적은 중립으로 표시 되는 흰색을 얻는 것입니다.

이는 사용자에 게 카메라 앞에 실제 회색 카드를 추가 하 고, `GrayWorldDeviceWhiteBalanceGains` 속성을 모니터링 하 고, 값이 세분화 될 때까지 대기 하도록 하는 메시지를 표시 하는 방식으로 응용 프로그램에서 구현할 수 있습니다.

그런 다음 응용 프로그램은 `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` `GrayWorldDeviceWhiteBalanceGains` 속성의 값을 사용 하 여 메서드에 대 한 균형 향상을 잠그고 변경 내용을 적용 합니다.

잠금 변경을 수행 하려면 먼저 캡처 장치를 구성에 대해 잠가야 합니다.

### <a name="manual-white-balance-example"></a>수동 흰색 잔액 예

일반 AV 캡처 설정 코드가 준비 되 면를 `UIViewController` 응용 프로그램의 스토리 보드에 추가 하 고 다음과 같이 구성할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image18.png "UIViewController는 응용 프로그램 스토리 보드에 추가 하 고 여기에 표시 된 대로 구성할 수 있습니다.")](intro-to-manual-camera-controls-images/image18.png#lightbox)

뷰에는 다음과 같은 주요 요소가 포함 되어 있습니다.

- 비디오 피드를 표시 하는입니다.`UIImageView`
- 포커스 모드를 자동에서 잠김으로 변경 하는입니다.`UISegmentedControl`
- 온도 `UISlider` 와 색조를 표시 하 고 업데이트 하는 두 개의 컨트롤
- 회색 카드 (회색 세계) 공간을 샘플링 하 고 해당 값을 사용 하 여 잔고를 설정 하는 데사용되는입니다.`UIButton`


수동 흰색 잔액 제어를 위해 보기 컨트롤러를 연결 하려면 다음을 수행 합니다.


1. 다음 using 문을 추가 합니다.

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using System.Timers;
    ```

1. 다음 전용 변수를 추가 합니다.

    ```csharp
    #region Private Variables
    private NSError Error;
    private bool Automatic = true;
    #endregion
    ```

1. 다음 계산 된 속성을 추가 합니다.

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    public Timer SampleTimer { get; set; }
    #endregion
    ```

1. 다음 전용 메서드를 추가 하 여 새로운 흰색 잔액 온도와 색조를 설정 합니다.

    ```csharp
    #region Private Methods
    void SetTemperatureAndTint() {
        // Grab current temp and tint
        var TempAndTint = new AVCaptureWhiteBalanceTemperatureAndTintValues (Temperature.Value, Tint.Value);

        // Convert Color space
        var gains = ThisApp.CaptureDevice.GetDeviceWhiteBalanceGains (TempAndTint);

        // Set the new values
        if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
            gains = NomralizeGains (gains);
            ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
            ThisApp.CaptureDevice.UnlockForConfiguration ();
        }
    }

    AVCaptureWhiteBalanceGains NomralizeGains (AVCaptureWhiteBalanceGains gains)
    {
        gains.RedGain = Math.Max (1, gains.RedGain);
        gains.BlueGain = Math.Max (1, gains.BlueGain);
        gains.GreenGain = Math.Max (1, gains.GreenGain);

        float maxGain = ThisApp.CaptureDevice.MaxWhiteBalanceGain;
        gains.RedGain = Math.Min (maxGain, gains.RedGain);
        gains.BlueGain = Math.Min (maxGain, gains.BlueGain);
        gains.GreenGain = Math.Min (maxGain, gains.GreenGain);

        return gains;
    }
    #endregion
    ```

1. 메서드를 `ViewDidLoad` 재정의 하 고 다음 코드를 추가 합니다.

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Set min and max values
        Temperature.MinValue = 1000f;
        Temperature.MaxValue = 10000f;

        Tint.MinValue = -150f;
        Tint.MaxValue = 150f;

        // Create a timer to monitor and update the UI
        SampleTimer = new Timer (5000);
        SampleTimer.Elapsed += (sender, e) => {
            // Convert color space
            var TempAndTint = ThisApp.CaptureDevice.GetTemperatureAndTintValues (ThisApp.CaptureDevice.DeviceWhiteBalanceGains);

            // Update slider positions
            Temperature.BeginInvokeOnMainThread (() => {
                Temperature.Value = TempAndTint.Temperature;
            });

            Tint.BeginInvokeOnMainThread (() => {
                Tint.Value = TempAndTint.Tint;
            });
        };

        // Watch for value changes
        Segments.ValueChanged += (sender, e) => {
            // Lock device for change
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {

                // Take action based on the segment selected
                switch (Segments.SelectedSegment) {
                case 0:
                // Activate auto focus and start monitoring position
                    Temperature.Enabled = false;
                    Tint.Enabled = false;
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.ContinuousAutoWhiteBalance;
                    SampleTimer.Start ();
                    Automatic = true;
                    break;
                case 1:
                // Stop auto focus and allow the user to control the camera
                    SampleTimer.Stop ();
                    ThisApp.CaptureDevice.WhiteBalanceMode = AVCaptureWhiteBalanceMode.Locked;
                    Automatic = false;
                    Temperature.Enabled = true;
                    Tint.Enabled = true;
                    break;
                }

                // Unlock device
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };

        // Monitor position changes
        Temperature.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        Tint.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Update white balance
            SetTemperatureAndTint ();
        };

        GrayCardButton.TouchUpInside += (sender, e) => {

            // If we are in the automatic mode, ignore changes
            if (Automatic)
                return;

            // Get gray card values
            var gains = ThisApp.CaptureDevice.GrayWorldDeviceWhiteBalanceGains;

            // Set the new values
            if (ThisApp.CaptureDevice.LockForConfiguration (out Error)) {
                ThisApp.CaptureDevice.SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains (gains, null);
                ThisApp.CaptureDevice.UnlockForConfiguration ();
            }
        };
    }
    ```

1. 메서드를 `ViewDidAppear` 재정의 하 고 다음을 추가 하 여 뷰가 로드 될 때 기록을 시작 합니다.

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
            SampleTimer.Start ();
        }
    }
    ```

1. 코드에 변경 내용을 저장 하 고 응용 프로그램을 실행 합니다.
1. 카메라를 Auto 모드로 사용 하면 카메라의 균형이 조정 됨에 따라 슬라이더가 자동으로 이동 됩니다.

    [![](intro-to-manual-camera-controls-images/image19.png "카메라가 흰색 밸런스를 조정할 때 슬라이더가 자동으로 이동 됩니다.")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. 잠긴 세그먼트를 탭 하 고 온도 슬라이더를 끌어서 흰색 밸런스를 수동으로 조정 합니다.

    [![](intro-to-manual-camera-controls-images/image20.png "온도 슬라이더를 끌어서 수동으로 조정 합니다.")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. 잠긴 세그먼트가 선택 된 상태에서 카메라 앞에 실제 회색 카드를 넣고 회색 카드 단추를 탭 하 여 회색 세계의 흰색 잔액을 조정 합니다.

    [![](intro-to-manual-camera-controls-images/image21.png "회색 카드 단추를 탭 하 여 회색 세계의 균형을 조정 합니다.")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. 응용 프로그램을 중지 합니다.

위의 코드는 카메라가 자동 모드에 있을 때 백색 잔액 설정을 모니터링 하는 방법 또는 슬라이더를 사용 하 여 잠금 모드에 있을 때의 밸런스를 제어 하는 방법을 보여 줍니다.

## <a name="bracketed-capture"></a>대괄호로 묶인 캡처

대괄호로 묶인 캡처는 위에 표시 된 수동 카메라 컨트롤의 설정을 기반으로 하며 응용 프로그램에서 다양 한 방식으로 시간을 캡처할 수 있도록 합니다.

간단히 말해서, 대괄호로 묶인 캡처는 그림에서 그림에 이르는 다양 한 설정을 사용 하 여 이미지를 버스트 하는 데 사용 됩니다.

[![](intro-to-manual-camera-controls-images/image22.png "대괄호로 묶인 캡처 작동 방식")](intro-to-manual-camera-controls-images/image22.png#lightbox)

응용 프로그램은 iOS 8에서 대괄호로 묶인 캡처를 사용 하 여 일련의 수동 카메라 컨트롤을 미리 설정 하 고, 단일 명령을 실행 하 고, 현재 장면이 각 수동 사전 설정에 대 한 일련의 이미지를 반환 하도록 할 수 있습니다.

### <a name="bracketed-capture-basics"></a>대괄호로 묶인 캡처 기본 사항

또한 대괄호로 묶인 캡처는 그림에서 그림으로 다양 한 설정으로 촬영 된 이미지의 버스트입니다. 사용할 수 있는 대괄호로 묶인 캡처 유형은 다음과 같습니다.

- **자동 노출 대괄호** – 모든 이미지의 편차 양이 달라 집니다.
- **수동 노출 대괄호** – 모든 이미지에 다양 한 셔터 속도 (기간) 및 ISO 금액이 있는 모든 이미지입니다.
- **단순 버스트 대괄호** – 연속적으로 연속 해 서 촬영 한 일련의 이미지입니다.


### <a name="new-bracketed-capture-controls-in-ios-8"></a>IOS 8의 새 대괄호로 묶인 캡처 컨트롤

대괄호로 묶인 모든 캡처 명령은 `AVCaptureStillImageOutput` 클래스에서 구현 됩니다. 지정 된 설정 배열을 사용 하 여 일련의 이미지를 가져오려면 메서드를사용합니다.`CaptureStillImageBracket`

설정을 처리 하기 위해 두 개의 새로운 클래스가 구현 되었습니다.

- `AVCaptureAutoExposureBracketedStillImageSettings`-자동 노출 괄호에 대 `ExposureTargetBias`한 바이어스를 설정 하는 데 사용 되는 속성이 하나 있습니다.
- `AVCaptureManual`  `ExposureBracketedStillImageSettings`-수동 노출 괄호에 대해 `ExposureDuration` 셔터 `ISO`속도와 ISO를 설정 하는 데 사용 되는 및 라는 두 가지 속성이 있습니다.


### <a name="bracketed-capture-controls-dos-and-donts"></a>대괄호로 묶인 캡처 컨트롤의 및 일과

#### <a name="dos"></a>Do

다음은 iOS 8에서 대괄호로 묶인 캡처 컨트롤을 사용할 때 수행 해야 하는 작업 목록입니다.

- 메서드를 `PrepareToCaptureStillImageBracket` 호출 하 여 최악의 캡처 상황을 대비해 앱을 준비 합니다.
- 샘플 버퍼가 동일한 공유 풀에서 제공 되는 것으로 가정 합니다.
- 이전 준비 호출에 의해 할당 된 메모리를 해제 하려면를 다시 호출 `PrepareToCaptureStillImageBracket` 하 고 하나의 개체 배열을 보냅니다.


#### <a name="donts"></a>일과

다음은 iOS 8에서 대괄호로 묶인 캡처 컨트롤을 사용할 때 수행 하지 않아야 하는 작업 목록입니다.

- 대괄호로 묶인 캡처 설정 유형을 단일 캡처로 혼합 하지 마세요.
- 단일 캡처에 이미지 `MaxBracketedCaptureStillImageCount` 를 초과 하 여 요청 하지 마세요.


### <a name="bracketed-capture-details"></a>대괄호로 묶인 캡처 세부 정보

IOS 8에서 대괄호로 묶인 캡처 작업을 수행할 때 다음 정보를 고려해 야 합니다.

- 대괄호로 묶인 설정은 `AVCaptureDevice` 설정을 일시적으로 재정의 합니다.
- Flash 및 여전히 이미지 안정화 설정은 무시 됩니다.
- 모든 이미지는 동일한 출력 형식 (jpeg, png 등)을 사용 해야 합니다.
- 비디오 미리 보기는 프레임을 삭제할 수 있습니다.
- 대괄호로 묶인 캡처는 iOS 8과 호환 되는 모든 장치에서 지원 됩니다.


이 정보를 염두에 두면 iOS 8에서 대괄호로 묶인 캡처를 사용 하는 예제를 살펴보겠습니다.

### <a name="bracket-capture-example"></a>대괄호 캡처 예제

일반 AV 캡처 설정 코드가 준비 되 면를 `UIViewController` 응용 프로그램의 스토리 보드에 추가 하 고 다음과 같이 구성할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image23.png "UIViewController는 응용 프로그램 스토리 보드에 추가 하 고 여기에 표시 된 대로 구성할 수 있습니다.")](intro-to-manual-camera-controls-images/image23.png#lightbox)

뷰에는 다음과 같은 주요 요소가 포함 되어 있습니다.

- 비디오 피드를 표시 하는입니다.`UIImageView`
- 캡처 `UIImageViews` 결과를 표시 하는 3입니다.
- `UIScrollView` 비디오 피드 및 결과 보기를 저장할입니다.
- 일부 기본 설정으로 대괄호로 묶인 캡처를 수행 하는 데사용되는입니다.`UIButton`


다음 작업을 수행 하 여 대괄호로 캡처하기 위해 뷰 컨트롤러를 연결 합니다.


1. 다음 using 문을 추가 합니다.

    ```csharp
    using System;
    using System.Drawing;
    using Foundation;
    using UIKit;
    using System.CodeDom.Compiler;
    using System.Collections.Generic;
    using System.Linq;
    using AVFoundation;
    using CoreVideo;
    using CoreMedia;
    using CoreGraphics;
    using CoreFoundation;
    using CoreImage;
    ```

1. 다음 전용 변수를 추가 합니다.

    ```csharp
    #region Private Variables
    private NSError Error;
    private List<UIImageView> Output = new List<UIImageView>();
    private nint OutputIndex = 0;
    #endregion
    ```

1. 다음 계산 된 속성을 추가 합니다.

    ```csharp
    #region Computed Properties
    public AppDelegate ThisApp {
        get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
    }
    #endregion
    ```

1. 다음 전용 메서드를 추가 하 여 필요한 출력 이미지 뷰를 빌드합니다.

    ```csharp
    #region Private Methods
    private UIImageView BuildOutputView(nint n) {

        // Create a new image view controller
        var imageView = new UIImageView (new CGRect (CameraView.Frame.Width * n, 0, CameraView.Frame.Width, CameraView.Frame.Height));

        // Load a temp image
        imageView.Image = UIImage.FromFile ("Default-568h@2x.png");

        // Add a label
        UILabel label = new UILabel (new CGRect (0, 20, CameraView.Frame.Width, 24));
        label.TextColor = UIColor.White;
        label.Text = string.Format ("Bracketed Image {0}", n);
        imageView.AddSubview (label);

        // Add to scrolling view
        ScrollView.AddSubview (imageView);

        // Return new image view
        return imageView;
    }
    #endregion
    ```

1. 메서드를 `ViewDidLoad` 재정의 하 고 다음 코드를 추가 합니다.

    ```csharp
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        // Hide no camera label
        NoCamera.Hidden = ThisApp.CameraAvailable;

        // Attach to camera view
        ThisApp.Recorder.DisplayView = CameraView;

        // Setup scrolling area
        ScrollView.ContentSize = new SizeF (CameraView.Frame.Width * 4, CameraView.Frame.Height);

        // Add output views
        Output.Add (BuildOutputView (1));
        Output.Add (BuildOutputView (2));
        Output.Add (BuildOutputView (3));

        // Create preset settings
        var Settings = new AVCaptureBracketedStillImageSettings[] {
            AVCaptureAutoExposureBracketedStillImageSettings.Create(-2.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(0.0f),
            AVCaptureAutoExposureBracketedStillImageSettings.Create(2.0f)
        };

        // Wireup capture button
        CaptureButton.TouchUpInside += (sender, e) => {
            // Reset output index
            OutputIndex = 0;

            // Tell the camera that we are getting ready to do a bracketed capture
            ThisApp.StillImageOutput.PrepareToCaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings,async (bool ready, NSError err) => {
                // Was there an error, if so report it
                if (err!=null) {
                    Console.WriteLine("Error: {0}",err.LocalizedDescription);
                }
            });

            // Ask the camera to snap a bracketed capture
            ThisApp.StillImageOutput.CaptureStillImageBracket(ThisApp.StillImageOutput.Connections[0],Settings, (sampleBuffer, settings, err) =>{
                // Convert raw image stream into a Core Image Image
                var imageData = AVCaptureStillImageOutput.JpegStillToNSData(sampleBuffer);
                var image = CIImage.FromData(imageData);

                // Display the resulting image
                Output[OutputIndex++].Image = UIImage.FromImage(image);

                // IMPORTANT: You must release the buffer because AVFoundation has a fixed number
                // of buffers and will stop delivering frames if it runs out.
                sampleBuffer.Dispose();
            });
        };
    }
    ```


1. 메서드를 `ViewDidAppear` 재정의 하 고 다음 코드를 추가 합니다.

    ```csharp
    public override void ViewDidAppear (bool animated)
    {
        base.ViewDidAppear (animated);

        // Start udating the display
        if (ThisApp.CameraAvailable) {
            // Remap to this camera view
            ThisApp.Recorder.DisplayView = CameraView;

            ThisApp.Session.StartRunning ();
        }
    }

    ```

1. 코드에 변경 내용을 저장 하 고 응용 프로그램을 실행 합니다.
1. 장면을 프레임으로 프레임 하 고 캡처 대괄호 단추를 누릅니다.

    [![](intro-to-manual-camera-controls-images/image24.png "장면을 프레임으로 프레임 하 고 캡처 대괄호 단추를 누릅니다.")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. 오른쪽에서 왼쪽으로 살짝 밀어 대괄호로 묶인 캡처에서 가져온 세 이미지를 확인 합니다.

    [![](intro-to-manual-camera-controls-images/image25.png "오른쪽에서 왼쪽으로 살짝 밀어 대괄호로 묶인 캡처에서 가져온 세 이미지를 확인 합니다.")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. 응용 프로그램을 중지 합니다.


위의 코드에서는 iOS 8에서 대괄호로 묶인 자동 노출을 구성 하 고 가져오는 방법을 보여 줍니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 8에서 제공 되는 새로운 수동 카메라 컨트롤을 소개 하 고, 수행 하는 작업과 작동 방법의 기본 사항에 대해 설명 했습니다. 수동 포커스, 수동 노출 및 수동 흰색 잔액의 예제를 제공 합니다. 마지막으로 앞에서 설명한 수동 카메라 컨트롤을 사용 하 여 대괄호로 묶인 캡처를 수행 하는 예제를 제공 했습니다.

## <a name="related-links"></a>관련 링크

- [ManualCameraControls (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/manualcameracontrols)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
