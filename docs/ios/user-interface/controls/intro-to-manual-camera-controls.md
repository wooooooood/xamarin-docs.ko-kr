---
title: Xamarin.iOS에서 수동 카메라 컨트롤
description: 이 문서에서는 iOS AVFoundation 프레임 워크를 사용할 수 있는 방법을 Xamarin.iOS를 사용 하 여 수동 카메라 컨트롤을 사용 하도록 설정 하려면 설명 합니다. 수동 카메라 컨트롤 컨트롤 포커스, 흰색 분산 및 노출 설정을 할을 수 있습니다.
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 84c4b699ba2c046eeb70963f3df71ca9a4760f3b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104182"
---
# <a name="manual-camera-controls-in-xamarinios"></a>Xamarin.iOS에서 수동 카메라 컨트롤

제공 하는 수동 카메라 컨트롤을 `AVFoundation Framework` ios 8에서 모바일 응용 프로그램이 iOS 장치의 카메라를 통해 완전 한 제어를 허용 합니다. 전문가 수준 카메라 응용 프로그램을 만들어 이미지 또는 비디오를 계속 수행 하는 동안 카메라의 매개 변수를 조정 하 여 아티스트 컴퍼지션을 제공이 세분화 된 수준의 제어를 사용할 수 있습니다.

과학 또는 산업용 응용 프로그램을 개발 하면 덜 맞춰진 정확성 또는 이미지의 장점은 않았고 맞춰진 보다 일부 기능이 나 사용 하는 이미지의 요소를 강조 표시 하는 경우에 이러한 컨트롤 유용할 수 있습니다.

## <a name="avfoundation-capture-objects"></a>AVFoundation 캡처 개체

같은지 여부를 비디오 또는 iOS 장치에서 카메라를 사용 하 여 이미지를 계속 수행, 해당 이미지를 캡처하는 데 사용 하는 프로세스 대부분입니다. 기본 자동 카메라 컨트롤 또는 새 수동 카메라 컨트롤을 활용 하는 것을 사용 하는 응용 프로그램의 그렇습니다.

 [![](intro-to-manual-camera-controls-images/image1.png "AVFoundation 캡처 개체 개요")](intro-to-manual-camera-controls-images/image1.png#lightbox)

입력에서 가져온 것을 `AVCaptureDeviceInput` 에 `AVCaptureSession` 의 방식으로 `AVCaptureConnection`합니다. 결과 여전히 이미지 또는 비디오 스트림으로 출력 중 하나입니다. 전체 프로세스에 의해 제어 되는 `AVCaptureDevice`합니다.

## <a name="manual-controls-provided"></a>제공 되는 수동 제어

IOS 8에서 제공 하는 새 Api를 사용 하는 응용 프로그램 다음 카메라 기능을 제어할을 사용할 수 있습니다.

-  **수동 포커스** – 응용 프로그램 직접 포커스를 제어 하려면 최종 사용자를 허용 하면 가져온 이미지 보다 자세히 제어를 제공할 수 있습니다.
-  **수동 노출** – 수동 노출 제어를 제공 하 여 응용 프로그램 사용자에 게 더 자유롭게 제공를 스타일이 지정 된 참조를 얻을 수 있도록 합니다.
-  **수동 흰색 균형** – 백서 분산 이미지에서 색을 조정 하는-현실적인 보이도록 하는 빈도입니다. 다른 광원은 다른 색 온도 있고 이러한 차이점에 대 한 보정을 위해 이미지를 캡처하는 데 사용 하는 카메라 설정을 조정 됩니다. 다시 사용자에 대 한 제어 흰색 분산 함으로써 사용자 조정을 자동으로 수행할 수 없는 가능 합니다.


기존 iOS Api 이미지가 세밀 하 게 제어할 수 있도록 향상 된 캡처 프로세스 및 iOS 8 확장을 제공 합니다.

## <a name="bracketed-capture"></a>대괄호로 묶은 캡처

대괄호로 묶은 캡처 위의 수동 카메라 컨트롤의 설정을 기반으로 하며 응용 프로그램을 잠시 시간 내에 다양 한 방식으로 캡처할 수 있습니다.

간단히 말해서 다양 한 그림 그림에서 설정이 계속 찍은 폭주는 캡처 대괄호로 묶습니다.

## <a name="requirements"></a>요구 사항

다음은이 기사에서 설명한 단계를 완료 하려면 필요 합니다.

-  **Xcode 7 이상 및 iOS 8 이상** – Apple의 Xcode 7 및 iOS 8 또는 최신 Api 설치 하 고 개발자의 컴퓨터에서 구성 해야 합니다.
-  **Mac 용 visual Studio** – 최신 버전의 Mac 용 Visual Studio를 설치 하 고 사용자 장치에 구성 해야 합니다.
-  **iOS 8 장치** – 최신 버전의 iOS 8 실행 하는 iOS 장치. IOS 시뮬레이터에서에서 카메라 기능을 테스트할 수 없습니다.


## <a name="general-av-capture-setup"></a>일반 AV 캡처 설치

IOS 장치에서 비디오 녹화 하는 경우에 다음과 같은 몇 가지 일반 설정 코드는 항상 필수입니다. 이 섹션에서는 iOS 장치의 카메라에서 비디오를 기록 하 고 해당 비디오를 표시 하는 데 필요한 최소한의 설치를 설명 하는 실시간으로 `UIImageView`합니다.

### <a name="output-sample-buffer-delegate"></a>출력 샘플 버퍼 대리자

샘플 출력 버퍼를 모니터링 하 여 버퍼에서 놓은 이미지를 표시 하는 대리자 됩니다 필요한 첫 번째 작업 중 하나는 `UIImageView` 응용 프로그램 UI입니다.

다음 루틴 샘플 버퍼 모니터링 되며 UI를 업데이트 합니다.

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

현재 위치에서이 루틴을 사용 하 여는 `AppDelegate` 라이브 비디오 피드를 기록할 AV 캡처 세션을 엽니다. 하도록 수정할 수 있습니다.

### <a name="creating-an-av-capture-session"></a>AV 캡처 세션 만들기

AV 캡처 세션 iOS 장치의 카메라에서 라이브 비디오 기록 제어 하는 데 사용 되 고 iOS 응용 프로그램에 비디오 하는 데 필요한 합니다. 이 예제에서는 이후 `ManualCameraControl` 샘플 응용 프로그램은 캡처 세션을 사용 하 여 여러 위치에서이 구성 하는 `AppDelegate` 전체 응용 프로그램에 사용할 수 있게 합니다.

응용 프로그램을 수정 하려면 다음을 수행 `AppDelegate` 필요한 코드를 추가 합니다.


1. 두 번 클릭 하 여 `AppDelegate.cs` 열어 편집 하려면 솔루션 탐색기에서 파일.
1. 다음 추가 파일의 맨 위에 문을 사용 하 여:
    
    ```
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

1. 계산 된 속성을 확인 하 고 다음 private 변수를 추가 합니다 `AppDelegate` 클래스:
    
    ```
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
  
1. 완료 메서드를 재정의 하 고로 변경 합니다.
    
    ```
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


이 코드를 사용 하 여 수동 카메라 컨트롤 실험 및 테스트에 쉽게 구현할 수 있습니다.

## <a name="manual-focus"></a>수동 포커스

최종 사용자가 포커스의 컨트롤을 직접 수행 함으로써 응용 프로그램 예술적인 제어할 가져온 이미지를 제공할 수 있습니다.

예를 들어는 전문 사진사 달성 하기 위해 이미지의 포커스를 출시할 수를 [빛 망 울 효과](http://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "빛 망 울 효과")](intro-to-manual-camera-controls-images/image2.png#lightbox)

또는 만듭니다는 [포커스 끌어오기 효과](http://www.mediacollege.com/video/camera/focus/pull.html), 같은:

[![](intro-to-manual-camera-controls-images/image3.png "포커스 끌어오기 효과")](intro-to-manual-camera-controls-images/image3.png#lightbox)

과학자 또는 의료 응용 프로그램 기록기에 대 한 응용 프로그램 프로그래밍 방식으로 이동 하 여 렌즈 실험에 대 한 좋습니다. 어느 방법이 든 새 API 최종 사용자 또는 응용 프로그램을 제어할 포커스 시 이미지 만들어질 수 있습니다.

### <a name="how-focus-works"></a>포커스의 작동 원리

전에 IOS 8 응용 프로그램에서 포커스를 제어 하는 데 대 한 세부 정보를 설명 합니다. 포커스 iOS 장치에서 작동 하는 방법을 빠르게 살펴보겠습니다.

[![](intro-to-manual-camera-controls-images/image4.png "IOS 장치에서 포커스의 작동 방식")](intro-to-manual-camera-controls-images/image4.png#lightbox)

IOS 장치에서 카메라 렌즈에 진입 빛은 이미지 센서에 중점을 합니다. 센서는 관계의 초점은 (이미지 표시 되는 위치는 기술로 영역) 인 렌즈 센서 컨트롤에서의 거리입니다. 센서에서 멀수록 렌즈는, 기술로 보일 거리 개체 및 개체 근처에 가까울수록 서로 기술로 보일

IOS 장치에서 렌즈 이동 가깝게 또는 멀리 센서 자석 및 springs 합니다. 결과적으로, 렌즈의 정확한 위치 불가능, 장치 마다 달라질 수 있으며 장치 방향 또는 장치와 spring 기간 같은 매개 변수에 따라 달라질 수 있습니다.

### <a name="important-focus-terms"></a>중요 한 초점은 용어

포커스를 처리할 때에 개발자에 게 익숙한 몇 가지 용어:

-  **필드의 깊이** – 가장 가까운 및 멀리 포커스 개체 사이의 거리입니다. 
-  **매크로** -포커스 스펙트럼의 거의 끝 이며 렌즈 집중할 수는 가장 가까운 거리입니다.
-  **무한대** – 포커스 스펙트럼의 맨 끝 이며 먼 거리는 렌즈 집중할 수 있습니다.
-  **Hyperfocal 거리** – 포커스의 맨 끝에 프레임에서 가장 멀리 개체가 포커스 스펙트럼의 지점입니다. 즉, 이것이 깊이의 필드를 최대화 하는 초점 위치입니다. 
-  **렌즈 위치** – 위의 모든 제어 대상을 용어입니다. 이 포커스의 컨트롤러는 센서에서 이동 하 고 있으므로 렌즈의 거리입니다.


이러한 용어와 염두에서 기술 자료를 사용 하 여 새 수동 포커스가 컨트롤 iOS 8 응용 프로그램에서 성공적으로 구현할 수 있습니다.

### <a name="existing-focus-controls"></a>기존 포커스가 컨트롤

iOS 7 및 이전 버전을 통해 기존 포커스가 컨트롤을 제공 `FocusMode`속성:

-   `AVCaptureFocusModeLocked` – 포커스가 포커스 단일 지점에서 잠겨 있습니다.
-   `AVCaptureFocusModeAutoFocus` -카메라 초점이 찾아 다음 상태를 유지 될 때까지 모든 초점 지점을 통해 렌즈를 추가 합니다.
-   `AVCaptureFocusModeContinuousAutoFocus` – 카메라에 포커스를 벗어난 상태를 감지할 때마다 refocuses 합니다.


기존 컨트롤을 통해 관심 설정할 수 있는 지점을 제공 합니다`FocusPointOfInterest` 속성인 사용자 특정 영역에 초점을 맞춰 탭 수 있도록 합니다. 응용 프로그램 모니터링 렌즈 이동을 추적할 수도 있습니다는 `IsAdjustingFocus` 속성입니다.

또한 범위 제한이 제공한는 `AutoFocusRangeRestriction` 속성:

-   `AVCaptureAutoFocusRangeRestrictionNear` –는 autofocus 주변 깊이를 제한합니다. QR 코드 또는 바코드를 스캔 같은 상황에서 유용 합니다.
-   `AVCaptureAutoFocusRangeRestrictionFar` –는 autofocus 먼 깊이를 제한합니다. 보기의 필드 (예를 들어 창 프레임)에서 개체는 무의미 한 것으로 알려져 있는 경우에 유용 합니다.


마지막은 `SmoothAutoFocus` 속성 자동 포커스 알고리즘 속도가 느려지고 비디오를 기록할 때 아티팩트를 이동 하지 않으려면 더 작은 단위로 단계입니다.

### <a name="new-focus-controls-in-ios-8"></a>IOS 8의에서 새 포커스 제어

이미 이상 iOS 7에서 제공 되는 기능을 하는 것 외에도 다음 기능은 이제 iOS 8에서에서 포커스를 제어할 수 있습니다:

-  수동 전체 컨트롤 포커스를 잠그는 경우 렌즈 위치입니다.
-  포커스 모드에서 렌즈 위치 키-값 관찰 합니다.


위의 기능을 구현 하는 `AVCaptureDevice` 클래스는 읽기 전용을 포함 하도록 수정 되었습니다 `LensPosition` 카메라 렌즈의 현재 위치를 가져오는 데 사용 되는 속성입니다.

렌즈 위치 수동 컨트롤을 사용 하려면 잠겨 포커스 모드에서 캡처 장치 여야 합니다. 예제:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

`SetFocusModeLocked` 카메라 렌즈의 위치를 조정 하려면 캡처 장치 메서드를 사용 합니다. 알림이 변경 내용을 적용 하는 경우에 선택적 콜백 루틴을 제공할 수 있습니다. 예제:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

위의 코드에서 볼 수 있듯이 렌즈 위치 변경 수 있으려면 먼저 캡처 장치 구성에 대 한 잠겨 있어야 합니다. 유효한 렌즈 위치 값은 0.0에서 1.0입니다.

### <a name="manual-focus-example"></a>수동 포커스 예제

현재 위치에서 일반 AV 캡처 설치 코드를 사용 하 여를 `UIViewController` 응용 프로그램의 스토리 보드에 추가할 수 있고 다음과 같이 구성 합니다.

[![](intro-to-manual-camera-controls-images/image5.png "UIViewController의 스토리 보드는 응용 프로그램에 추가 하 고 여기 표시 된 대로 구성 수 있습니다.")](intro-to-manual-camera-controls-images/image5.png#lightbox)

보기에는 다음 주요 요소가 포함 됩니다.

-  `UIImageView` 비디오 피드를 표시 하는 됩니다.
-  `UISegmentedControl` 는 포커스 모드에서 자동으로 바뀝니다 잠금.
-  `UISlider` 표시 되며 현재 렌즈 위치 업데이트입니다.


선 위로 뷰 컨트롤러 수동 포커스가 컨트롤에 대 한 다음을 수행 합니다.


1. 다음 추가 문을 사용 하 여:

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
  
1. 다음 private 변수를 추가 합니다.

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
  
1. 재정의 `ViewDidLoad` 메서드 다음 코드를 추가 합니다.

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
  
1. 재정의 `ViewDidAppear` 메서드 뷰가 로드 되 면 기록을 시작 하려면 다음을 추가 합니다.

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
  
1. 자동 모드에서 카메라를 사용 하 여 슬라이더 카메라 포커스를 조정 하는 대로 자동으로 이동 합니다.

    [![](intro-to-manual-camera-controls-images/image6.png "이 샘플 앱에서 포커스를 조정 하는 카메라도 슬라이더는 자동으로 이동")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. 잠금 세그먼트를 탭 하 고 렌즈 위치를 수동으로 조정 하려면 슬라이더 위치:

    [![](intro-to-manual-camera-controls-images/image7.png "렌즈 위치를 수동으로 조정")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. 응용 프로그램을 중지 합니다.


위의 코드 렌즈 위치 카메라 자동 모드에 있을 때 모니터링 잠금 모드에 있을 때 렌즈 위치를 제어 하는 슬라이더를 사용 하는 방법을 보여 주었습니다.

## <a name="manual-exposure"></a>수동 노출

노출 원본 밝기를 기준으로 이미지의 밝기를 나타내고 긴 및 센서 (ISO 매핑) 게인 수준 도달 하는 방법에 대 한 센서에서 빛의 양을 결정 됩니다. 수동 노출 제어를 제공 하 여 응용 프로그램 최종 사용자에 게 더 자유롭게 제공 하 고 스타일 모양 허용할 수 있습니다.

수동 노출을 컨트롤을 사용 하는 사용자 어둡게 및 moody 유출이 밝은에서 이미지를 사용할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image8.png "샘플에 어둡게 및 moody 유출이 밝은 노출을 보여 주는 이미지")](intro-to-manual-camera-controls-images/image8.png#lightbox)

마찬가지로이 가능 과학 응용 프로그램 또는 응용 프로그램 사용자 인터페이스에서 제공 하는 수동 컨트롤을 통해 프로그래밍 방식 제어를 자동으로 사용 합니다. 어느 방법이 든 새 iOS 8 노출을 Api은 카메라의 노출 설정 세부적으로 제어를 제공 합니다.

### <a name="how-exposure-works"></a>노출의 작동 원리

전에 IOS 8 응용 프로그램에 대 한 노출을 제어의 세부 정보를 설명 합니다. 개요 노출의 작동 원리를 살펴보겠습니다.

[![](intro-to-manual-camera-controls-images/image9.png "노출의 작동 원리")](intro-to-manual-camera-controls-images/image9.png#lightbox)

세 가지 기본 요소 노출을 제어 하 함께 제공 되는 다음과 같습니다.

-  **속도 셔터** – 셔터 빛의 카메라 센서에 있도록 열려 있는 시간의 길이입니다. 짧은 셔터 열려 있는 경우 시간 적은 광원은 허용 하 고는 선명한 이미지는 (작은 동작 흐림 효과). 길수록 셔터를 열지 더 많이 발생 하는 흐림 효과 동작 및 자세한 조명 사용 수 있습니다.
-  **ISO 매핑** – 필름 사진에서 가져온 용어임 이며 빛 영화에서는 화학 물질의 민감도 가리킵니다. 영화에서는 낮은 ISO 값 있고 작은 수준이 finer 색 재생; 디지털 센서에서 낮은 ISO 값 어둡게 하지만 센서 노이즈가 경우 ISO 값이 높을수록, 밝을수록 이미지 했지만 센서 노이즈를 더 많이 있습니다. "ISO" 디지털 센서에 대 한 측정입니다 [electronic 향상](http://en.wikipedia.org/wiki/Gain), 물리적 기능 하지 않습니다. 
-  **Aperture 렌즈** – 렌즈 열기의 크기입니다. 모든 iOS 장치에서 렌즈 구경 고정 되어 있으므로 노출 조정에 사용할 수 있는 두 개의 값은 셔터 속도 및 ISO입니다.


### <a name="how-continuous-auto-exposure-works"></a>연속 하는 방법을 자동 노출 작동

학습 하기 전에 수동 노출 작동 방식를 연속 하는 방법을 자동 노출 하는 iOS 장치에서 작동 합니다.

[![](intro-to-manual-camera-controls-images/image10.png "IOS 장치에서 작동 하는 연속 자동 노출 하는 방법")](intro-to-manual-camera-controls-images/image10.png#lightbox)

먼저 자동 노출을 블록은 이상적인 노출을 계산 작업이 고 지속적으로 계량 통계 공급 하기는 합니다. 이 정보를 사용 하 여 최적의 켜지 지 잘 장면 ISO 고 셔터 속도의 혼합을 계산 합니다. 이 주기를 AE 루프 이라고 합니다.

### <a name="how-locked-exposure-works"></a>잠긴된 어떻게 노출 작동

그런 다음 iOS 장치에서 작동 하는 노출 하는 방법이 잠긴된를 검토해 보겠습니다.

[![](intro-to-manual-camera-controls-images/image11.png "잠긴 iOS 장치에서 작동 하는 노출 하는 방법")](intro-to-manual-camera-controls-images/image11.png#lightbox)

마찬가지로 최적의 iOS 및 Duration 값을 계산 하는 자동 노출을 차단 해야 합니다. 그러나이 모드에서는 AE 블록에서에서 연결이 끊어진 계량 통계 엔진입니다.

### <a name="existing-exposure-controls"></a>기존 노출 제어

iOS 7 이상을 통해 다음과 같은 기존 노출을 컨트롤을 제공 합니다 `ExposureMode` 속성:

-   `AVCaptureExposureModeLocked` – 장면을 한 번 샘플링 하 고 장면 전체에서 해당 값을 사용 하 여 키를 누릅니다.
-   `AVCaptureExposureModeContinuousAutoExposure` – 잘 켜져 있는지 확인 하는 지속적으로 장면을 샘플입니다.


합니다 `ExposurePointOfInterest` 장면에 노출 하는 대상 개체를 선택 하 여 노출 하려면 탭을 사용할 수 있으며 응용 프로그램을 모니터링할 수는 `AdjustingExposure` 속성을 노출 조절 될 경우.

### <a name="new-exposure-controls-in-ios-8"></a>IOS 8의에서 새 노출 제어

이미 이상 iOS 7에서 제공 되는 기능을 하는 것 외에도 다음 기능은 이제 iOS 8에에서 대 한 노출을 제어 하는 사용 가능한:

-  완벽 하 게 수동 사용자 지정 노출 됩니다.
-  Get, Set, 키-값 IOS 및 셔터 (기간)를 확인합니다.


위의 기능을 구현 하는 새 `AVCaptureExposureModeCustom` 모드가 추가 되었습니다. 카메라를 사용자 지정 모드 경우 노출을 기간 및 ISO를 조정 하려면 다음 코드를 사용할 수 있습니다.

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

자동 및 잠금 모드에서는 응용 프로그램 다음 코드를 사용 하 여 자동 루틴에 대 한 바이어스를 조정할 수 있습니다.

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

최소 및 최대 설정 범위는 되지 하드 코딩 되어야 하므로 응용 프로그램이 실행 중인 장치에 따라 달라 집니다. 다음 속성을 최소 및 최대 값 범위를 가져오려면 대신 사용 합니다.

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


위의 코드에서 볼 수 있듯이 노출 변경 수 있으려면 먼저 캡처 장치 구성에 대 한 잠겨 있어야 합니다.

### <a name="manual-exposure-example"></a>수동 노출 예제

현재 위치에서 일반 AV 캡처 설치 코드를 사용 하 여를 `UIViewController` 응용 프로그램의 스토리 보드에 추가할 수 있고 다음과 같이 구성 합니다.

[![](intro-to-manual-camera-controls-images/image12.png "UIViewController의 스토리 보드는 응용 프로그램에 추가 하 고 여기 표시 된 대로 구성 수 있습니다.")](intro-to-manual-camera-controls-images/image12.png#lightbox)

보기에는 다음 주요 요소가 포함 됩니다.

-  `UIImageView` 비디오 피드를 표시 하는 됩니다.
-  `UISegmentedControl` 는 포커스 모드에서 자동으로 바뀝니다 잠금.
-  4 개의 `UISlider` 표시 되며 오프셋, 기간, ISO 및 편차를 업데이트 하는 컨트롤입니다.


선 위로 뷰 컨트롤러 수동 Exposure Control에 대 한 다음을 수행 합니다.


1. 다음 추가 문을 사용 하 여:
    
    ```
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
  
1. 다음 private 변수를 추가 합니다.

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
  
1. 재정의 `ViewDidLoad` 메서드 다음 코드를 추가 합니다.

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
  
1. 재정의 `ViewDidAppear` 메서드 뷰가 로드 되 면 기록을 시작 하려면 다음을 추가 합니다.

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
  
1. 자동 모드에서 카메라를 사용 하 여 슬라이더 카메라 조정 노출 하는 대로 자동으로 이동 합니다.

    [![](intro-to-manual-camera-controls-images/image13.png "카메라 노출을 조정으로 슬라이더는 자동으로 이동")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. 잠금 세그먼트를 탭 하 고 슬라이더를 끌어 바이어스를 자동 노출에 대 한 바이어스를 수동으로 조정:

    [![](intro-to-manual-camera-controls-images/image14.png "자동 노출에 대 한 바이어스를 수동으로 조정")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. 사용자 지정 세그먼트를 탭 하 고 수동으로 노출을 제어 하는 기간 및 ISO 슬라이더를 드래그 합니다.

    [![](intro-to-manual-camera-controls-images/image15.png "수동으로 노출 제어에 기간 및 ISO 슬라이더를 드래그 합니다.")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. 응용 프로그램을 중지 합니다.


위의 코드는 자동 모드에서는 카메라 경우 노출을 설정을 모니터링 하는 방법을 보여 주었습니다 및 슬라이더를 사용 하 여 잠금 또는 사용자 지정 모드에 있을 때 표시를 제어 하는 방법입니다.

## <a name="manual-white-balance"></a>수동 흰색 잔액

흰색 균형 컨트롤 colosr 더 사실적 수 있도록 이미지의 균형을 조정 하는 작업을 할 수 있습니다. 다른 광원은 다른 색 온도 있고 이러한 차이점에 대 한 보정을 위해 이미지를 캡처하는 데 사용 하는 카메라 설정을 조정 해야 합니다. 다시 흰색 분산을 통해 사용자 컨트롤을 허용 하 여 수의 조정을 전문 꾸밈 효과를 얻기 위해 자동 루틴의 지원 되지 않는 합니다.

[![](intro-to-manual-camera-controls-images/image16.png "수동 흰색 균형 조정을 보여 주는 샘플 이미지를")](intro-to-manual-camera-controls-images/image16.png#lightbox)

예를 들어, 일광 절약 텅스텐 형광등 광원 워 머, 주황색 노랑 색조 있지만 blueish 캐스트를 있습니다. (스럽지만, "쿨" 색 경우 "웜" 색 보다 더 높은 색 온도 색 온도 물리적 측정값을 지 단일에 없습니다.)

사용자 마음 색 온도 간의 차이 보정 뛰어난 이지만 카메라를 수행할 수 없는 것입니다. 카메라 색 간의 차이 조정 하는 반대 스펙트럼의 색을 승격 하 여 작동 합니다.

새 iOS 8 노출을 API를 통해 응용 프로그램 프로세스의 제어에 카메라의 흰색 분산 설정에 대 한 세분화 된 제어를 제공 합니다.

### <a name="how-white-balance-works"></a>흰색 어떻게 분산 작동

전에 IOS 8 응용 프로그램의 흰색 균형을 제어 하는 데 대 한 세부 정보를 설명 합니다. 신속 하 게 확인 하는 방법을 흰색 분산 작동을 살펴보겠습니다.

색상 관념 연구에는 [CIE 1931 RGB 색 공간 및 CIE 1931 XYZ 색 공간](http://en.wikipedia.org/wiki/CIE_1931_color_space) 첫 번째 색 공간을 수학적으로 정의 됩니다. 1931에서 국제 Commission 조명을 (CIE)에 의해 생성 되었습니다.

[![](intro-to-manual-camera-controls-images/image17.png "CIE 1931 RGB 색 공간 및 CIE 1931 XYZ 색 공간")](intro-to-manual-camera-controls-images/image17.png#lightbox)

위의 차트 표시의 모든 색 밝은 빨강, 녹색 밝은 파란색에서 사람의 눈에 표시 됩니다. 위의 그래프에 표시 된 대로 X 및 Y 값을 사용 하 여 다이어그램의 시점을 그릴 수 있습니다.

그래프에 표시로 휴먼 비전의 범위를 벗어난 것을 그래프에 표시할 수 있는 X 및 Y 값 및 결과적으로 카메라에서 이러한 색을 재현할 수 없는 합니다.

위의 차트에서 작은 곡선 이라고 합니다 [Planckian 궤적](http://en.wikipedia.org/wiki/Planckian_locus)색 온도 (도 켈빈) (더울) 파란색 우변 많으면를 표현 하는, 및 빨간색 우변 (울지) 더 낮은 숫자입니다. 이러한 일반적인 조명 상황에 유용합니다.

혼합된 조명 조건에서 필요한 변경을 수행 하려면 Planckian 궤적에서 벗어날 흰색 균형 조정 해야 합니다. 조정 해야 하는 이러한 상황에서를 이동 하거나 녹색 또는 CIE 빨간색/자홍 변의 확장 합니다.

iOS 장치 반대 색 향상을 승격 하 여 색 캐스트에 대 한 보정 합니다. 예를 들어, 장면 너무 많이 있으면 빨간색 향상 보정을 위해 상승 됩니다. 이러한 향상을 장치에 종속 되므로 값이 특정 장치에 대 한 보정 됩니다.

### <a name="existing-white-balance-controls"></a>기존 흰색 분산 컨트롤

iOS 7 이상를 통해 다음과 같은 기존 흰색 균형 컨트롤을 제공 하 고 `WhiteBalanceMode` 속성:

-   `AVCapture WhiteBalance ModeLocked` – 한 번 장면 및 장면 전체에서 해당 값을 사용 하 여 샘플링 합니다.
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` – 잘 분산 되도록 지속적으로 장면을 샘플입니다.


응용 프로그램을 모니터링할 수는 `AdjustingWhiteBalance` 속성을 노출 조절 될 경우을 참조 하세요.

### <a name="new-white-balance-controls-in-ios-8"></a>IOS 8의에서 새로운 백서 분산 컨트롤

이미 이상 iOS 7에서 제공 되는 기능을 하는 것 외에도 다음 기능은 이제 iOS 8에서에서 흰색 분산 컨트롤에서 사용 가능한:

-  RGB 장치를 완벽 하 게 수동 제어 권한을 얻습니다.
-  Get, Set, 키-값 관찰 장치 RGB 얻게 됩니다.
-  회색 카드를 사용 하 여 흰색 균형을 지원 합니다.
-  장치 독립 색 공간에서 변환 루틴입니다.


위의 기능을 구현 하는 `AVCaptureWhiteBalanceGain` 구조는 다음과 같은 멤버를 사용 하 여 추가 되었습니다.

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


최대 흰색 분산 향상 되어 4 개에서 준비할 수 있습니다 하 고는 `MaxWhiteBalanceGain` 속성입니다. 따라서 (1)에서 올바른 범위는 `MaxWhiteBalanceGain` (4) 현재 합니다.

`DeviceWhiteBalanceGains` 속성의 현재 값을 확인에 사용할 수 있습니다. 사용 하 여 `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` 균형을 조정 하려면 카메라 잠긴된 흰색 분산 모드에 있을 때 얻습니다.

#### <a name="conversion-routines"></a>변환 루틴

IOS 8 to 및 from과으로 변환 하는 데에 변환 루틴을 추가한 장치 독립적인 색 공간입니다. 변환 루틴을 구현 하는 `AVCaptureWhiteBalanceChromaticityValues` 구조는 다음과 같은 멤버를 사용 하 여 추가 되었습니다.

-   `X` -0에서 1 사이의 값입니다.
-   `Y` -0에서 1 사이의 값입니다.


`AVCaptureWhiteBalanceTemperatureAndTintValues` 구조는 다음과 같은 멤버를 사용 하 여 추가 되었습니다.

-   `Temperature` -부동 소수점 값 켈빈 합니다.
-   `Tint` -녹색 및 자홍 0에서 150으로 녹색 방향 양수 값을 사용 하 여는 자홍에서 음수 쪽으로 오프셋입니다.


사용 된 `CaptureDevice.GetTemperatureAndTintValues`및 `CaptureDevice.GetDeviceWhiteBalanceGains`온도 tint, chromaticity RGB 간의 변환 방법 색 공간을 확보 합니다.

> [!NOTE]
> 변환 루틴은 가까울수록 Planckian 궤적 변환할 값이 더 정확 합니다.




#### <a name="gray-card-support"></a>회색 카드 지원

Apple는 iOS 8에 내장 된 회색 카드 지원 가리키도록 회색 World 용어를 사용 합니다. 사용자를 중심 프레임의 50% 이상에서는를 사용 하는 백서 균형을 조정 하는 실제 회색 카드에 집중할 수 있습니다. 중립 표시 되는 백서를 달성 하기 위해 회색 카드의 목적은입니다.

이 물리적 회색 카드 카메라 앞에 추가할 사용자에 게 응용 프로그램에서 구현할 수 있습니다 모니터링을 `GrayWorldDeviceWhiteBalanceGains` 속성과 값 정착 될 때까지 대기 합니다.

응용 프로그램에 대 한 백서 분산 향상에 잠금이 수행 됩니다는 `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` 메서드는 값을 사용 하는 `GrayWorldDeviceWhiteBalanceGains` 속성 변경 내용을 적용 합니다.

흰색 분산 변경 수 있으려면 먼저 구성에 대 한 캡처 장치를 잠가야 합니다.

### <a name="manual-white-balance-example"></a>수동 흰색 분산 예제

현재 위치에서 일반 AV 캡처 설치 코드를 사용 하 여를 `UIViewController` 응용 프로그램의 스토리 보드에 추가할 수 있고 다음과 같이 구성 합니다.

[![](intro-to-manual-camera-controls-images/image18.png "UIViewController의 스토리 보드는 응용 프로그램에 추가 하 고 여기 표시 된 대로 구성 수 있습니다.")](intro-to-manual-camera-controls-images/image18.png#lightbox)

보기에는 다음 주요 요소가 포함 됩니다.

-  `UIImageView` 비디오 피드를 표시 하는 됩니다.
-  `UISegmentedControl` 는 포커스 모드에서 자동으로 바뀝니다 잠금.
-  두 `UISlider` 표시 되며 온도 및 색조를 업데이트 하는 컨트롤입니다.
-  `UIButton` 회색 카드 (회색 World) 공간 샘플 및 해당 값을 사용 하 여 흰색 분산을 설정 하는 데 사용 합니다.


실시간 접속 뷰 컨트롤러 수동 흰색 분산 제어 하려면 다음을 수행 합니다.


1. 다음 추가 문을 사용 하 여:

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
  
1. 다음 private 변수를 추가 합니다.

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
  
1. 새 백서를 설정 하려면 다음 개인 메서드를 추가 온도 및 Tint 균형을 유지 합니다.

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
  
1. 재정의 `ViewDidLoad` 메서드 다음 코드를 추가 합니다.

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
  
1. 재정의 `ViewDidAppear` 메서드 뷰가 로드 되 면 기록을 시작 하려면 다음을 추가 합니다.

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
  
1. 변경 내용을 저장 합니다 코드 및 응용 프로그램을 실행 합니다.
1. 자동 모드에서 카메라를 사용 하 여 슬라이더 카메라 흰색 균형을 조정 하는 대로 자동으로 이동 합니다.

    [![](intro-to-manual-camera-controls-images/image19.png "카메라 흰색 균형 조정으로 슬라이더는 자동으로 이동")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. 잠금 세그먼트를 탭 하 고 수동으로 흰색 균형을 조정 하려면 Temp 및 Tint 슬라이더를 끌어:

    [![](intro-to-manual-camera-controls-images/image20.png "수동으로 흰색 균형을 조정 하려면 Temp 및 Tint 슬라이더를 끌어")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. 여전히 선택한 잠긴 세그먼트를 사용 하 여 카메라의 앞에 물리적 회색 카드 놓고 회색 전 세계에 흰색 균형을 조정 하려면 회색 카드 단추를 탭 합니다.

    [![](intro-to-manual-camera-controls-images/image21.png "회색 전 세계에 흰색 균형을 조정 하려면 회색 카드 단추를 탭")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. 응용 프로그램을 중지 합니다.

위의 코드는 카메라 자동 모드에 있을 때 흰색 밸런스 설정이 모니터링 하거나 슬라이더를 사용 하 여 잠금 모드에 있을 때 흰색 균형을 제어 하는 방법을 보여 주었습니다.

## <a name="bracketed-capture"></a>대괄호로 묶은 캡처

대괄호로 묶은 캡처 위의 수동 카메라 컨트롤의 설정을 기반으로 하며 응용 프로그램을 잠시 시간 내에 다양 한 방식으로 캡처할 수 있습니다.

간단히 말해서 다양 한 그림 그림에서 설정이 계속 찍은 폭주는 캡처 대괄호로 묶습니다.

[![](intro-to-manual-camera-controls-images/image22.png "대괄호로 묶은 캡처의 작동 방식")](intro-to-manual-camera-controls-images/image22.png#lightbox)

대괄호로 묶은 캡처를 사용 하 여 ios 8, 응용 프로그램 수 일련의 수동 카메라 컨트롤을 미리 설정, 단일 명령을 실행 있고 현재 장면 수동 사전 설정 중 각각에 대 한 일련의 이미지를 반환 합니다.

### <a name="bracketed-capture-basics"></a>대괄호로 묶은 캡처 기본 사항

다시 캡처 대괄호로 묶은 여전히 찍은 사진 그림에서 다양 한 설정을 사용 하 여 폭주는입니다. 대괄호로 묶은 캡처의 형식은 다음과 같습니다.

-  **노출 괄호를 자동** 모든 이미지는 다양 한 바이어스 크기를 해야 하는 위치입니다.
-  **수동 노출을 대괄호** 모든 이미지는 다양 한 셔터 (기간) 및 ISO 있는 – 크기입니다.
-  **간단한 버스트 대괄호** – 일련의 빠르게 연속적으로 수행 하는 이미지도 합니다.


### <a name="new-bracketed-capture-controls-in-ios-8"></a>대괄호로 묶은 캡처의 새 제어 iOS 8

모든 캡처 대괄호로 묶은 명령에서 구현 되는 `AVCaptureStillImageOutput` 클래스입니다. 사용 된 `CaptureStillImageBracket`메서드 설정의 지정 된 배열 사용 하 여 일련의 이미지를 합니다.

설정을 처리 하려면 두 개의 새 클래스가 구현 되었습니다.

-   `AVCaptureAutoExposureBracketedStillImageSettings` – 속성이 하나 `ExposureTargetBias`는 자동 노출 대괄호에 대 한 바이어스를 설정 하는 데 사용 합니다. 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` –에 두 개의 속성인 `ExposureDuration` 및 `ISO`수동 노출 대괄호에 대 한 ISO와 셔터 속도 설정 하는 데 사용 합니다. 


### <a name="bracketed-capture-controls-dos-and-donts"></a>대괄호로 묶은 캡처 일과 제어합니다.

#### <a name="dos"></a>할 것

다음은 iOS 8에서에서 컨트롤을 대괄호로 묶은 캡처를 사용할 때 수행 되어야 할 사항의 목록입니다.

-  호출 하 여 해당 캡처 최악의 상황에 맞는 앱을 준비 합니다 `PrepareToCaptureStillImageBracket` 메서드.
-  동일한 공유 풀에서 제공 하는 샘플 버퍼 것임을 가정 합니다.
-  준비에 대 한 이전 호출에 의해 할당 된 메모리를 해제 하려면 호출 `PrepareToCaptureStillImageBracket` 다시 보내는 한 개체의 배열입니다.


#### <a name="donts"></a>하지 말아야 할 사항

다음은 iOS 8에서에서 컨트롤을 대괄호로 묶은 캡처를 사용할 때 수행할 수 해야 하는 사항의 목록입니다.

-  단일 캡처에 설정 형식을 대괄호로 묶은 캡처를 혼합 하지 마세요.
-  요청 하지 둘 `MaxBracketedCaptureStillImageCount` 단일 캡처 이미지입니다.


### <a name="bracketed-capture-details"></a>대괄호로 묶은 캡처 세부 정보

다음 세부 정보를 캡처 대괄호로 묶은 iOS 8에서 작업할 때 고려해 야 취해야 합니다.

-  대괄호로 묶은 설정을 일시적으로 재정의 된 `AVCaptureDevice` 설정 합니다.
-  Flash 및 여전히 이미지 안정화 설정은 무시 됩니다.
-  모든 이미지는 동일한 출력 형식 (예: jpeg, png)을 사용 해야 합니다.
-  비디오 미리 보기 프레임을 삭제할 수 있습니다.
-  대괄호로 묶은 캡처는 iOS 8과 호환 되는 모든 장치에서 지원 됩니다.


이 정보를 염두에서를 사용 하 여 iOS 8에에서 대괄호로 묶은 캡처를 사용 하는 예제를 살펴보겠습니다.

### <a name="bracket-capture-example"></a>괄호 캡처 예제

현재 위치에서 일반 AV 캡처 설치 코드를 사용 하 여를 `UIViewController` 응용 프로그램의 스토리 보드에 추가할 수 있고 다음과 같이 구성 합니다.

[![](intro-to-manual-camera-controls-images/image23.png "UIViewController의 스토리 보드는 응용 프로그램에 추가 하 고 여기 표시 된 대로 구성 수 있습니다.")](intro-to-manual-camera-controls-images/image23.png#lightbox)

보기에는 다음 주요 요소가 포함 됩니다.

-  `UIImageView` 비디오 피드를 표시 하는 됩니다.
-  세 가지 `UIImageViews` 는 캡처의 결과 표시 합니다.
-  `UIScrollView` 비디오 피드 및 결과 뷰를 수용 하도록 합니다.
-  `UIButton` 미리 설정 된 일부 설정을 사용 하 여 대괄호로 묶은 캡처를 만드는 데 사용 합니다.


선 위로 뷰 컨트롤러 캡처용으로 대괄호로 묶은 다음을 수행 합니다.


1. 다음 추가 문을 사용 하 여:

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
  
1. 다음 private 변수를 추가 합니다.

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
  
1. 필요한 출력 이미지 뷰 작성 하는 다음 개인 메서드를 추가 합니다.

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
  
1. 재정의 `ViewDidLoad` 메서드 다음 코드를 추가 합니다.
    
    ```
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
    
  
1. 재정의 `ViewDidAppear` 메서드 다음 코드를 추가 합니다.

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
    
1. 변경 내용을 저장 합니다 코드 및 응용 프로그램을 실행 합니다.
1. 프레임 장면 및 괄호 캡처 단추를 탭 합니다.

    [![](intro-to-manual-camera-controls-images/image24.png "장면 프레임 및 괄호 캡처 단추를 탭 합니다.")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. 오른쪽에서 왼쪽으로 대괄호로 묶은 캡처에 의해 수행 된 세 개의 이미지를 보려면 살짝 밉니다.

    [![](intro-to-manual-camera-controls-images/image25.png "오른쪽에서 왼쪽으로 대괄호로 묶은 캡처에 의해 수행 된 세 개의 이미지를 보려면 살짝 밀기")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. 응용 프로그램을 중지 합니다.


위의 코드를 구성 하 고 iOS 8에는 자동 노출을 대괄호로 묶은 캡처를 수행 하는 방법을 보여 주었습니다.

## <a name="summary"></a>요약

이 문서의 iOS 8에서 제공 하는 새 수동 카메라 컨트롤에 간략하게 설명 하 고 수행할 작업 및 작동 하는 방법의 기본을 다루었습니다. 수동 포커스, 수동 노출 및 수동 흰색 잔액 예가 지정 합니다. 예를 대괄호로 묶은 캡처 설명한 수동 카메라 컨트롤을 사용 하 여 수행 했던 마지막으로,

## <a name="related-links"></a>관련 링크

- [ManualCameraControls (샘플)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
