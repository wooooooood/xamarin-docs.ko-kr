---
title: 수동 카메라 컨트롤
description: AVFoundation 프레임 워크를 사용 하면 사용자가 수동 카메라 컨트롤에 대 한 허용 하 여 멋진 사진을 하기가 매우 편리 합니다. 이 프레임 워크를 사용 하 여 응용 프로그램 카메라 포커스, 흰색 균형 및 노출 설정을 직접 제어할을 걸릴 수 있습니다. 응용 프로그램를 자동으로 다른 노출 설정 사용 하 여 이미지를 캡처할 대괄호로 묶은 노출 캡처 사용할 수도 있습니다. 이 문서를 수동 카메라 컨트롤을 사용 하 여 간단한 iOS 8 모바일 응용 프로그램에서 개요를 걸립니다.
ms.prod: xamarin
ms.assetid: 56340225-5F3C-4BFC-9A79-61496D7FE5B5
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 8545dce1b9232e396c4c9e71ad5f20649eef2417
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="manual-camera-controls"></a>수동 카메라 컨트롤

_AVFoundation 프레임 워크를 사용 하면 사용자가 수동 카메라 컨트롤에 대 한 허용 하 여 멋진 사진을 하기가 매우 편리 합니다. 이 프레임 워크를 사용 하 여 응용 프로그램 카메라 포커스, 흰색 균형 및 노출 설정을 직접 제어할을 걸릴 수 있습니다. 응용 프로그램를 자동으로 다른 노출 설정 사용 하 여 이미지를 캡처할 대괄호로 묶은 노출 캡처 사용할 수도 있습니다. 이 문서를 수동 카메라 컨트롤을 사용 하 여 간단한 iOS 8 모바일 응용 프로그램에서 개요를 걸립니다._

제공 하는 수동 카메라 컨트롤은 `AVFoundation Framework` iOS 8, iOS 장치의 카메라를 통해 완전히 제어 하는 모바일 응용 프로그램을 허용 합니다. 이 세분화 된 수준의 제어 여전히 이미지 나 비디오 촬영 하는 동안 카메라의 매개 변수를 조정 하 여 아티스트 컴포지션 설명과 전문 수준 카메라 응용 프로그램을 만드는 데 사용할 수 있습니다.

과학 및 산업 응용 프로그램을 개발, 여기서 결과 덜 맞춰진 정확성 또는 이미지의 장점은 고 맞춰진 보다 몇 가지 기능 또는 수행 되는 이미지의 요소를 강조 표시 하는 경우에 이러한 컨트롤 유용할 수 있습니다.

## <a name="avfoundation-capture-objects"></a>AVFoundation 캡처 개체

비디오 또는 여전히 iOS 장치에서 카메라를 사용 하 여 이미지를 가져와 여부를 이러한 이미지를 캡처하는 데 사용 프로세스는 대부분 동일 합니다. 기본 자동 카메라 컨트롤 또는 새 수동 카메라 컨트롤을 활용 하는 스토리를 사용 하는 응용 프로그램에 해당 됩니다.

 [![](intro-to-manual-camera-controls-images/image1.png "AVFoundation 캡처 개체 개요")](intro-to-manual-camera-controls-images/image1.png#lightbox)

입력에서 가져온 것는 `AVCaptureDeviceInput` 에 `AVCaptureSession` 통해는 `AVCaptureConnection`합니다. 결과 여전히 이미지 또는 비디오 스트림으로 출력 중 하나입니다. 전체 프로세스에 의해 제어 되는 `AVCaptureDevice`합니다.

## <a name="manual-controls-provided"></a>제공 되는 수동 제어

IOS 8에서 제공 된 새 Api를 사용 하 여 응용 프로그램의 카메라 기능 제어를 사용할 수 있습니다.

-  **수동 포커스** – 포커스를 직접 제어할 수 최종 사용자를 허용 하 여 응용 프로그램 보다 상세하게 수행 되는 이미지를 제공할 수 있습니다.
-  **수동 노출** –의 가능성을 수동으로 제어를 제공 하 여 응용 프로그램 수 원하는 대로 사용자에 게 제공 하 고 할 스타일 모양입니다.
-  **수동 흰색 밸런스** – 흰색 밸런스는 이미지의 색상을 조정 하는 데-현실적인 보이도록 할 빈도를 합니다. 다른 광원은 다른 색 온도 있고 카메라 설정 이미지를 캡처하는 데 이러한 차이 대 한 보정을 위해 조정 됩니다. 다시 흰색 균형 대 한 사용자 제어를 허용 하면 사용자가 조정할 수으로 자동으로 수행할 수 없습니다.


기존 iOS 이미지가 세부적으로 제어를 제공 하기 위한 Api에 대 한 향상 캡처 프로세스 및 iOS 8 확장을 제공 합니다.

## <a name="bracketed-capture"></a>대괄호로 묶은 캡처

대괄호로 묶은 캡처 위에 수동 카메라 컨트롤에서 설정에 기반 하 고 응용 프로그램을 여러 가지 다른 방법으로 시점에에서 캡처를 허용 합니다.

간단히 말하면 여전히 찍은 다양 한 설정 그림에서 그림의 캡처 대괄호로 묶습니다.

## <a name="requirements"></a>요구 사항

다음은이 문서에 제공 된 단계를 완료 하려면 필요 합니다.

-  **Xcode 7 이상 및 iOS 8 이상이** – Apple의 Xcode 7 및 iOS 8 또는 최신 Api을 설치 하 고 개발자의 컴퓨터에 구성 해야 합니다.
-  **Mac 용 visual Studio** -최신 버전의 Mac 용 Visual Studio를 설치 하 고 사용자 장치에 구성 해야 합니다.
-  **iOS 8 장치** – iOS 장치에서 최신 버전의 iOS 8 실행 합니다. IOS 시뮬레이터에서에서 카메라 기능을 테스트할 수 없습니다.


## <a name="general-av-capture-setup"></a>일반 AV 캡처 설치

IOS 장치에서 비디오를 기록 하는 경우에 다음과 같은 몇 가지 일반적인 설치 코드 항상 필요한 것은입니다. 이 섹션은 비디오 iOS 장치의 카메라를 기록 하 고 해당 비디오에서 표시 하는 데 필요한 최소한의 설치를 설명에서 실시간는 `UIImageView`합니다.

### <a name="output-sample-buffer-delegate"></a>출력 샘플 버퍼 대리자

샘플 출력 버퍼를 모니터링 하 고 버퍼의 잡을 이미지를 표시 하는 대리자 될 필요는 첫 번째 작업 중 하나는 `UIImageView` 응용 프로그램 UI에에서 있습니다.

다음 루틴 샘플 버퍼를 모니터링 하 고 UI가 업데이트 됩니다.

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

이 루틴을 제자리에 사용 된 `AppDelegate` 한 AV 캡처 세션을 라이브 비디오 피드를 기록 하도록 수정할 수 있습니다.

### <a name="creating-an-av-capture-session"></a>AV 캡처 세션을 만드는

AV 캡처 세션의 라이브 비디오 iOS 장치 카메라에서 기록을 제어할 사용 되 고 화면이 표시 iOS 응용 프로그램으로 하는 데 필요한. 이 예제에서는 이후 `ManualCameraControl` 에서 구성 됩니다, 샘플 응용 프로그램은 여러 위치에서 캡처 세션을 사용 하는 `AppDelegate` 전체 응용 프로그램에 사용할 수 있게 합니다.

응용 프로그램의 수정 하려면 다음을 수행 `AppDelegate` 필요한 코드를 추가 합니다.


1. 두 번 클릭 하 고 `AppDelegate.cs` 편집 하기 위해 열려는 솔루션 탐색기에서 파일입니다.
1. 다음 코드를 추가 하는 파일의 맨 위에 문을 사용 하 여:
    
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

1. 다음과 같은 개인 변수 및 계산 된 속성을 추가 `AppDelegate` 클래스:
    
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
  
1. 완성 된 메서드를 재정의 하 고로 변경 합니다.
    
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


이 코드가 수동 카메라 컨트롤 실험 및 테스트에 쉽게 구현할 수 있습니다.

## <a name="manual-focus"></a>수동 포커스

컨트롤의 포커스를 직접 적용 하려면 최종 사용자를 허용 하 여 응용 프로그램 보다 꾸밈 제어할 수행 되는 이미지를 제공할 수 있습니다.

예를 들어 전문 작가 달성 하기 위해 이미지의 포커스를 부드럽게 수는 [Bokeh 효과](http://en.wikipedia.org/wiki/Bokeh):

[![](intro-to-manual-camera-controls-images/image2.png "Bokeh 효과")](intro-to-manual-camera-controls-images/image2.png#lightbox)

또는 만들는 [포커스 끌어오기 효과](http://www.mediacollege.com/video/camera/focus/pull.html), 같은:

[![](intro-to-manual-camera-controls-images/image3.png "포커스 끌어오기 효과")](intro-to-manual-camera-controls-images/image3.png#lightbox)

과학자 또는 의료 응용 프로그램의 작성자에 대 한 응용 프로그램 수 프로그래밍 방식으로 실험 렌즈를 이동 하려고 합니다. 어떤 방법을 사용 하는 새로운 API 이미지 때 포커스에 대 한 제어를 적용 하려면 응용 프로그램 또는 최종 사용자가 수행 하는 것을 허용 합니다.

### <a name="how-focus-works"></a>포커스의 작동 원리

IOS 8 응용 프로그램에서 포커스 제어의 세부 정보를 설명 하기 전에 합니다. 포커스 iOS 장치에서 작동 하는 방법을 빠르게 살펴보겠습니다.

[![](intro-to-manual-camera-controls-images/image4.png "IOS 장치에서 포커스의 작동 방식")](intro-to-manual-camera-controls-images/image4.png#lightbox)

밝은 테마 이미지 센서에 포커스가가 iOS 장치에서 카메라 렌즈를 입력 합니다. 여기서는 중점이 (이미지 표시 되는 위치는 가장 선명한 영역), 관계에는 센서에 렌즈 센서 컨트롤에서의 거리입니다. 멀리 렌즈를 확인 하는 센서에서 거리 개체 가장 선명 하 게 보일 있고, 개체 근처에 가깝게 울 가장 선명 하 게 합니다.

IOS 장치에서 렌즈 이동 곧 제공 될 받는 사람 또는 센서 먼 자석과 및 springs 합니다. 결과적으로, 렌즈의 정확한 위치 수 없으면, 장치의 방향 또는 장치와 스프링 세 매개 변수에 의해 영향을 받을 수 있으며 장치 마다 달라 집니다.

### <a name="important-focus-terms"></a>중요 한 포커스 용어

포커스를 처리할 때에 개발자와 파악 해야 하는 몇 가지 용어:

-  **필드의 깊이** – 가장 가까운 및 가장 멀리 포커스 개체 사이의 거리입니다. 
-  **매크로** -포커스 스펙트럼의 반대 쪽 끝 쪽에 있는 이며 렌즈 집중할 수 있는 가장 가까운 거리입니다.
-  **무한대** – 이것은 포커스 스펙트럼의 맨 끝으로 먼 거리 렌즈 집중할 수입니다.
-  **Hyperfocal 거리** – 위치나 포커스의 맨 끝에 프레임에서 가장 멀리 있는 개체가 포커스 스펙트럼의 지점이입니다. 즉,이 필드의 깊이 최대화 하는 초점 위치입니다. 
-  **위치 렌즈** -위의 모든 제어 된 다음 용어입니다. 이 포커스의 컨트롤러 센서에서 있고, 따라서 렌즈의 거리입니다.


이러한 용어 및 기술 염두에서에 새 수동 포커스 컨트롤 iOS 8 응용 프로그램에서 성공적으로 구현할 수 있습니다.

### <a name="existing-focus-controls"></a>기존 포커스 컨트롤

iOS 7 및 이전 버전을 통해 기존 포커스가 컨트롤을 제공 `FocusMode`속성:

-   `AVCaptureFocusModeLocked` – 포커스 포커스 단일 지점에서 잠겨 있습니다.
-   `AVCaptureFocusModeAutoFocus` – 카메라 초점이 찾아 다음 그대로 보존 될 때까지 모든 중요 부분은 통해 렌즈를 비우고 합니다.
-   `AVCaptureFocusModeContinuousAutoFocus` – 카메라는 포커스를 벗어난 상태를 감지할 때마다 refocuses 합니다.


기존 컨트롤에도 관심을 통해 설정할 수 있는 지점을 제공는`FocusPointOfInterest` 속성을 사용자가 특정 영역에 초점을 얻을 수 있도록 합니다. 응용 프로그램 모니터링 하 여 렌즈 이동을 추적할 수도 `IsAdjustingFocus` 속성입니다.

또한 범위 제한 제공한는 `AutoFocusRangeRestriction` 속성:

-   `AVCaptureAutoFocusRangeRestrictionNear` –는 autofocus 근처 깊이를 제한합니다. QR 코드 또는 바코드를 스캔 같은 상황에서 유용 합니다.
-   `AVCaptureAutoFocusRangeRestrictionFar` –는 autofocus 먼 깊이를 제한합니다. 무의미 한 것으로 알려진 개체 보기의 필드 (예를 들어, 창 프레임)에서 모두 경우에 유용 합니다.


마지막으로 `SmoothAutoFocus` 속성 자동 포커스 알고리즘 속도 낮추고 및 비디오를 기록할 때 아티팩트를 이동 하지 않으려면 더 작은 증가값으로 단계입니다.

### <a name="new-focus-controls-in-ios-8"></a>새 iOS 8 포커스 제어

Ios 7 이상과 이미 제공 되는 기능 외에 다음과 같은 기능을 사용 iOS 8에에서 포커스를 제어할 수 이제:

-  포커스를 잠글 때 렌즈 위치의 전체 수동 제어입니다.
-  모든 포커스 모드에서 렌즈 위치의 키-값 관찰 합니다.


위의 기능을 구현 하는 `AVCaptureDevice` 클래스는 읽기 전용 포함 하도록 수정 되었습니다 `LensPosition` 카메라 렌즈의 현재 위치를 가져오는 데 사용 되는 속성입니다.

수동 렌즈 위치 제어 하려면, 캡처 장치 잠긴 포커스 모드에 있어야 합니다. 예제:

 `CaptureDevice.FocusMode = AVCaptureFocusMode.Locked;`

`SetFocusModeLocked` 카메라 렌즈의 위치를 조정 하려면 캡처 장치 메서드를 사용 합니다. 변경 내용이 적용 하는 경우 알림을 가져오기 위한 선택적 콜백 루틴을 제공할 수 있습니다. 예제:

```csharp
ThisApp.CaptureDevice.LockForConfiguration(out Error);
ThisApp.CaptureDevice.SetFocusModeLocked(Position.Value,null);
ThisApp.CaptureDevice.UnlockForConfiguration();
```

위의 코드에서와 같이 렌즈 위치 변경 수 있으려면 먼저 캡처 장치 구성에 대 한 잠겨 있어야 합니다. 유효한 렌즈 위치 값은 0.0에서 1.0입니다.

### <a name="manual-focus-example"></a>수동 포커스 예제

현재 위치에서 일반 AV 캡처 설치 코드와는 `UIViewController` 응용 프로그램의 스토리 보드에 추가 하 고 다음과 같이 구성할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image5.png "UIViewController는 스토리 보드 응용 프로그램에 추가 하 고 여기 표시 된 대로 구성 수 있습니다.")](intro-to-manual-camera-controls-images/image5.png#lightbox)

뷰는 다음과 같은 주요 요소가 포함 되어 있습니다.

-  A `UIImageView` 비디오 피드에 표시할 합니다.
-  A `UISegmentedControl` 잠금을 포커스 모드에서 자동 변경 됩니다.
-  A `UISlider` 를 표시 하 고 현재 렌즈 위치를 업데이트 합니다.


와이어 접속 뷰 컨트롤러 수동 포커스 컨트롤에 대 한 다음을 실행 합니다.


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
  
1. 다음과 같은 전용 변수를 추가 합니다.

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
  
1. 재정의 `ViewDidLoad` 메서드를 다음 코드를 추가 합니다.

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
  
1. 재정의 `ViewDidAppear` 메서드 보기 로드 될 때 기록을 시작 하려면 다음 줄을 추가 합니다.

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
  
1. Auto 모드에서 카메라로 슬라이더 카메라 포커스를 조정 하는 대로 자동으로 이동 합니다.

    [![](intro-to-manual-camera-controls-images/image6.png "이 샘플 응용 프로그램에 포커스를 조정 하는 카메라도 슬라이더는 자동으로 이동")](intro-to-manual-camera-controls-images/image6.png#lightbox)
1. 잠금 세그먼트 누르고 렌즈 위치를 수동으로 조정 하려면 슬라이더 위치가 끕니다.

    [![](intro-to-manual-camera-controls-images/image7.png "수동으로 렌즈 위치를 조정")](intro-to-manual-camera-controls-images/image7.png#lightbox)
1. 응용 프로그램을 중지 합니다.


위의 코드는 카메라 자동 모드에 있을 때 렌즈 위치를 모니터링 하거나 잠금 모드일 때 렌즈 위치를 제어 하는 슬라이더를 사용 하는 방법으로 나타났습니다.

## <a name="manual-exposure"></a>수동 노출

노출 소스 밝기와 관련 된 이미지의 밝기 나타내며 및 센서 (ISO 매핑) 게인 수준으로 도달 방법에 대 한 센서 얼마나 조명 하 여 결정 됩니다. 수동으로 제어할의 가능성을 제공 하 여 응용 프로그램 최종 사용자에 게 보다 자유롭게를 제공 하 고 있는 스타일이 적용 된 모양 하도록 허용할 수 있습니다.

수동 노출 컨트롤을 사용 하는 사용자 어두운 및 moody 비현실적으로 밝은에서 이미지를 사용할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image8.png "샘플 어두운과 moody 비현실적으로 밝은 노출을 보여 주는 이미지")](intro-to-manual-camera-controls-images/image8.png#lightbox)

다시, 이렇게 과학 응용 프로그램 또는 응용 프로그램 사용자 인터페이스에서 제공 되는 수동 제어를 통해 프로그래밍 방식 제어를 사용 하 여 자동으로 합니다. 어떤 방법을 사용 하는 새 iOS 8 노출 Api은 카메라의 노출 설정에 대 한 세분화 된 제어를 제공 합니다.

### <a name="how-exposure-works"></a>노출 작동 방식

IOS 8 응용 프로그램에 노출 제어의 세부 정보를 설명 하기 전에 합니다. 노출의 작동 원리 빠른 살펴보겠습니다.

[![](intro-to-manual-camera-controls-images/image9.png "노출 작동 방식")](intro-to-manual-camera-controls-images/image9.png#lightbox)

노출 제어 하려면 함께 제공 하는 세 가지 기본 요소는:

-  **속도 셔터** – 셔터 light 카메라 센서에 있도록 열려 있는 시간 길이입니다. 짧은 셔터 열릴 시간이 덜 light 사용은 및 보여줍니다는 이미지의 크기는 (덜 동작 흐림 효과). 길수록 셔터 열려 있으며, 빛이 더 많이 및 더 발생 하는 흐림 효과 동작에 사용 됩니다.
-  **ISO 매핑** – 용어 필름 사진에서 가져온 것 이므로 빛 필름에 화학 물질의 민감도 가리킵니다. 덜 세분화 및 색상 충실도 더욱 세밀 하 게; 필름 낮은 ISO 값 필요 디지털 센서에서 낮은 ISO 값 어둡게 하지만 덜 센서 노이즈를 있습니다. ISO 값이 클수록, 밝을수록 이미지 있지만 더 많은 센서 노이즈가 있는 합니다. 디지털 센서에 "ISO"는 측정 [전자 이득](http://en.wikipedia.org/wiki/Gain)는 실제 기능이 아니라 합니다. 
-  **구멍 렌즈** – 렌즈를 확인 하 여 크기입니다. 모든 iOS 장치에서 렌즈 구멍 고정 되어 있으므로 셔터 속도 및 ISO 노출 조정에 사용할 수 있는 두 개의 값은입니다.


### <a name="how-continuous-auto-exposure-works"></a>어떻게 연속 자동 노출 작동

학습 하기 전에 수동 노출 작동 방법 이기는 확인 된 iOS 장치에서 작동 하는 역할을 어떻게 연속 자동 노출 이해 해야 합니다.

[![](intro-to-manual-camera-controls-images/image10.png "IOS 장치에서 작동 하는 연속 자동 노출 하는 방법")](intro-to-manual-camera-controls-images/image10.png#lightbox)

먼저 자동 노출 블록은 이상적인 노출을 계산 작업을 포함 하 고 지속적으로 공급 되는 방향이 계량 통계. 이 정보를 사용 하 여 최적의 혼합 잘 켜지 장면 가져오려는 ISO 및 셔터 속도 계산 합니다. 이 주기 AE 루프 라고 합니다.

### <a name="how-locked-exposure-works"></a>어떻게 잠긴된 노출 작동

그런 다음 iOS 장치에서 어떻게 잠긴된 노출 works에 살펴보겠습니다.

[![](intro-to-manual-camera-controls-images/image11.png "잠긴 iOS 장치에서 작동 하는 노출")](intro-to-manual-camera-controls-images/image11.png#lightbox)

다시, 최적의 iOS 및 기간 값을 계산 하려고 하는 자동 노출 블록 해야 합니다. 그러나이 모드에서는 AE 블록에서에서 연결이 끊어진 계량 통계 엔진입니다.

### <a name="existing-exposure-controls"></a>기존 노출 컨트롤

iOS 7 이상을 통해 다음과 같은 기존 노출 컨트롤을 제공 하 고는 `ExposureMode` 속성:

-   `AVCaptureExposureModeLocked` – 장면을 한 번 샘플링 하 고 장면 전체에서 이러한 값을 사용 합니다.
-   `AVCaptureExposureModeContinuousAutoExposure` – 장면 잘 켜지 확인에 지속적으로 샘플링 합니다.


`ExposurePointOfInterest` 장면에 노출 하는 대상 개체를 선택 하 여 노출 하려면 탭을 사용할 수 있으며 응용 프로그램을 모니터링할 수는 `AdjustingExposure` 속성을 노출 조절 될 경우를 참조 하십시오.

### <a name="new-exposure-controls-in-ios-8"></a>IOS 8 새 노출 제어

Ios 7 이상과 이미 제공 되는 기능 외에 다음과 같은 기능을 사용 iOS 8에에서 대 한 노출을 제어할 수 이제:

-  완전 하 게 수동 사용자 지정 노출 합니다.
-  Get, Set, 키-값에는 IOS 및 셔터 속도 (기간)을 확인합니다.


위의 기능을 구현 하려면 새 `AVCaptureExposureModeCustom` 모드 추가 되었습니다. 카메라의 사용자 지정 모드 이면 다음 코드 노출 기간과 ISO를 조정 해야 사용할 수 있습니다.:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.LockExposure(DurationValue,ISOValue,null);
CaptureDevice.UnlockForConfiguration();
```

자동 및 잠금 모드에서 응용 프로그램의 다음 코드를 사용 하 여 자동 노출 루틴 바이어스를 조정할 수 있습니다.:

```csharp
CaptureDevice.LockForConfiguration(out Error);
CaptureDevice.SetExposureTargetBias(Value,null);
CaptureDevice.UnlockForConfiguration();
```

최소값 및 최대값 설정을 범위는 하지 하드 코딩 해야 하므로, 응용 프로그램이 실행 되는 장치에 따라 다릅니다. 대신, 다음 속성을 사용 하 여 최소 및 최대 값 범위를 가져오려는:

-   `CaptureDevice.MinExposureTargetBias` 
-   `CaptureDevice.MaxExposureTargetBias` 
-   `CaptureDevice.ActiveFormat.MinISO` 
-   `CaptureDevice.ActiveFormat.MaxISO` 
-   `CaptureDevice.ActiveFormat.MinExposureDuration` 
-   `CaptureDevice.ActiveFormat.MaxExposureDuration` 


위의 코드에서와 같이 노출 변경 수 있으려면 먼저 캡처 장치 구성에 대 한 잠겨 있어야 합니다.

### <a name="manual-exposure-example"></a>수동 노출 예제

현재 위치에서 일반 AV 캡처 설치 코드와는 `UIViewController` 응용 프로그램의 스토리 보드에 추가 하 고 다음과 같이 구성할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image12.png "UIViewController는 스토리 보드 응용 프로그램에 추가 하 고 여기 표시 된 대로 구성 수 있습니다.")](intro-to-manual-camera-controls-images/image12.png#lightbox)

뷰는 다음과 같은 주요 요소가 포함 되어 있습니다.

-  A `UIImageView` 비디오 피드에 표시할 합니다.
-  A `UISegmentedControl` 잠금을 포커스 모드에서 자동 변경 됩니다.
-  4 개의 `UISlider` 표시 되며 오프셋, 기간, ISO 및 바이어스를 업데이트 하는 컨트롤입니다.


와이어 접속 뷰 컨트롤러 수동 Exposure Control에 대 한 다음을 실행 합니다.


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
  
1. 다음과 같은 전용 변수를 추가 합니다.

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
  
1. 재정의 `ViewDidLoad` 메서드를 다음 코드를 추가 합니다.

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
  
1. 재정의 `ViewDidAppear` 메서드 보기 로드 될 때 기록을 시작 하려면 다음 줄을 추가 합니다.

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
  
1. Auto 모드에서 카메라로 슬라이더 카메라 조정 노출 하는 대로 자동으로 이동 합니다.

    [![](intro-to-manual-camera-controls-images/image13.png "카메라 노출 조정으로 슬라이더는 자동으로 이동")](intro-to-manual-camera-controls-images/image13.png#lightbox)
1. 잠금 세그먼트를 누르고 자동 노출 바이어스를 수동으로 조정 하려면 바이어스 슬라이더를 끕니다.

    [![](intro-to-manual-camera-controls-images/image14.png "자동 노출 바이어스를 수동으로 조정")](intro-to-manual-camera-controls-images/image14.png#lightbox)
1. 사용자 지정 세그먼트를 수동으로 노출 제어 기간 및 ISO 슬라이더를 끕니다.

    [![](intro-to-manual-camera-controls-images/image15.png "수동으로 노출 제어를 기간 및 ISO 슬라이더를 끌어 옵니다.")](intro-to-manual-camera-controls-images/image15.png#lightbox)
1. 응용 프로그램을 중지 합니다.


위의 코드는 것으로 나타났습니다 카메라 자동 모드에 있을 때 노출 설정을 모니터링 하는 방법 및 잠금 또는 사용자 지정 모드에 있을 때 노출을 제어 하려면 슬라이더를 사용 하는 방법입니다.

## <a name="manual-white-balance"></a>수동 화이트 밸런스

흰색 밸런스 컨트롤에서 이미지를 좀 더 현실적인 보이도록 colosr 균형을 조정 하는 사용자 수 있습니다. 다른 광원은 다른 색 온도 있고 카메라 설정 이미지를 캡처하는 데 이러한 차이 대 한 보정을 위해 조정 해야 합니다. 다시 흰색 균형 대 한 사용자 제어를 허용 하 여 전문적인 조정을 자동 루틴은 지원 되지 않는의 꾸밈 효과를 얻기 위해 내릴 수 있습니다.

[![](intro-to-manual-camera-controls-images/image16.png "수동 흰색 균형 조정을 보여 주는 샘플 이미지")](intro-to-manual-camera-controls-images/image16.png#lightbox)

예를 들어, 일광 절약 텅스텐 incandescent lights 워 머, 노랑 주황색 tint 반면 blueish 캐스트를 있습니다. (사용,: "쿨" 색 "웜" 색 보다 더 높은 색 온도 있습니다. 색 온도 물리적 측정값을 정도가 하나를 사용 하지 않습니다.)

휴먼 주의 색 온도의 차이점에 대 한 보정을 이지만 카메라 작업할 수 없는 것입니다. 카메라 색 차이점을 조정 하는 반대 스펙트럼에서 색을 승격 하 여 작동 합니다.

새 iOS 8 노출 API를 통해 응용 프로그램을 하는 프로세스를 제어 하 고 카메라의 흰색 균형 설정에 대 한 세분화 된 제어를 제공 합니다.

### <a name="how-white-balance-works"></a>어떻게 흰색 균형 작동

흰색 균형 IOS 8 응용 프로그램에서 제어의 세부 정보를 설명 하기 전에 합니다. 빠르게 살펴보는 방법을 흰색 균형 works 보겠습니다.

색상 관념의 연구에는 [CIE 1931 RGB 색 공간 및 CIE 1931 XYZ 색 공간](http://en.wikipedia.org/wiki/CIE_1931_color_space) 는 첫 번째 색 공간을 수학적으로 정의 합니다. 원래 1931 국제 Commission 조명 (CIE)에 의해 작성 된 합니다.

[![](intro-to-manual-camera-controls-images/image17.png "CIE 1931 RGB 색 공간 및 CIE 1931 XYZ 색 공간")](intro-to-manual-camera-controls-images/image17.png#lightbox)

위의 차트 표시 모든 색 밝은 빨강, 녹색 밝은 파란색에서 사람의 눈에 표시 됩니다. 다이어그램에서 언제 든 위의 그래프에 표시 된 것 처럼 X 및 Y 값을 그릴 수 있습니다.

그래프에 표시는 인간 비전의 범위를 벗어나는 것 그래프에 그릴 수 있는 값이 X 및 Y로 이러한 색 카메라 하 여 재현할 수 없는 결과적으로 합니다.

위의 차트에서 작은 곡선 라고는 [Planckian 궤적](http://en.wikipedia.org/wiki/Planckian_locus)는 표현 (hotter) 파란색 쪽 번호가 높은 색 온도 (에 켈빈) 및 (냉각기) 변에 빨간색 아래 번호입니다. 이들은 일반적인 조명 상황에 유용 합니다.

혼합된 조명 조건에서 흰색 균형 조정 작업을 실행 하 고 필요한 사항을 변경 하려면 Planckian 궤적 해야 합니다. 이러한 경우 조정 해야 합니다 되도록 이동 중 녹색 또는 빨간색/자홍 쪽은 CIE의 크기를 조정 합니다.

색상 캐스트에 대 한 반대 색 이득 승격 하 여 iOS 장치를 보정 합니다. 예를 들어, 장면에 너무 많이 있는 경우 빨간색 이득 보정을 위해 승격 됩니다. 이러한 향상 장치에 종속 되기 때문에 특정 장치에 대 한 값 보정 됩니다.

### <a name="existing-white-balance-controls"></a>기존 흰색 균형 컨트롤

iOS 7 이상를 통해 다음과 같은 기존 흰색 밸런스 컨트롤을 제공 하 고 `WhiteBalanceMode` 속성:

-   `AVCapture WhiteBalance ModeLocked` – 장면에 한 번 및 장면 전반에서 해당 값을 사용 하 여 샘플링 합니다.
-   `AVCapture WhiteBalance ModeContinuousAutoExposure` – 장면 잘 분산 하려면 지속적으로 샘플링 합니다.


응용 프로그램을 모니터링할 수는 `AdjustingWhiteBalance` 속성을 노출 조절 될 경우를 참조 하십시오.

### <a name="new-white-balance-controls-in-ios-8"></a>IOS 8 새 흰색 잔액 제어

Ios 7 이상과 이미 제공 되는 기능 외에 다음과 같은 기능 iOS 8에에서 흰색 밸런스 컨트롤에서 사용 가능한 이제 됩니다.

-  장치 RGB 완벽 하 게 수동 제어 권한을 얻습니다.
-  Get, Set, 키-값을 관찰 RGB 장치를 획득합니다.
-  회색 카드를 사용 하 여 흰색 균형에 대 한 지원입니다.
-  장치 독립적 색 공간에서 변환 루틴입니다.


위의 기능을 구현 하는 `AVCaptureWhiteBalanceGain` 구조는 다음 멤버로 구성 된 추가 되었습니다.

-   `RedGain` 
-   `GreenGain` 
-   `BlueGain` 


최대 흰색 균형 이득이 작을 현재 4 (4)에서 사용할 수 있습니다 및는 `MaxWhiteBalanceGain` 속성입니다. 에 하나 (1)에서 올바른 범위는 `MaxWhiteBalanceGain` (4) 현재 합니다.

`DeviceWhiteBalanceGains` 현재 값을 확인 하려면 속성을 사용할 수 있습니다. 사용 하 여 `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` 카메라 잠긴된 흰색 균형 모드에 있을 때에 향상 균형을 조정 합니다.

#### <a name="conversion-routines"></a>변환 루틴

변환 루틴에서 to, 변환에 도움이 되도록 iOS 8에 추가 된 장치 독립적인 색 공간입니다. 변환 루틴을 구현 하는 `AVCaptureWhiteBalanceChromaticityValues` 구조는 다음 멤버로 구성 된 추가 되었습니다.

-   `X` -는 0에서 1 사이의 값입니다.
-   `Y` -는 0에서 1 사이의 값입니다.


`AVCaptureWhiteBalanceTemperatureAndTintValues` 구조는 다음 멤버로 구성 된 추가 되었습니다.

-   `Temperature` -형식이 부동 소수점 값 켈빈 합니다.
-   `Tint` -녹색 또는 자홍 0에서 150으로 녹색 방향 양수 값을 갖는 및는 자홍색 음의 방향으로 오프셋입니다.


사용 하 여는 `CaptureDevice.GetTemperatureAndTintValues`및 `CaptureDevice.GetDeviceWhiteBalanceGains`온도 tint, chromaticity RGB 간의 변환을 위한 메서드를 색 공간을 얻을 수 있습니다.

> [!NOTE]
> 변환 루틴은 Planckian 궤적 변환할 값이 가까워집니다 더 정확 합니다.




#### <a name="gray-card-support"></a>회색 카드 지원

Apple 회색 세계 용어를 사용 하 여 iOS 8에 기본 제공 되는 회색 카드 지원 기능은 참조 합니다. 흰색 균형을 조정 하려면이 사용 하 여 프레임의 중심의 최소 50%를 포함 하는 실제 회색 카드에 초점을 맞출 수 있습니다. 회색 카드의 목적은 중립 나타나는 흰색 달성 하는 것입니다.

이 물리적 회색 카드 카메라 앞에 배치 하려면 사용자에 게 응용 프로그램에서 구현할 수 있습니다 모니터링는 `GrayWorldDeviceWhiteBalanceGains` 속성 및 값 문제 해결을 위한 될 때까지 대기 합니다.

응용 프로그램에 대 한 흰색 밸런스 향상을 잠글 다음는 `SetWhiteBalanceModeLockedWithDeviceWhiteBalanceGains` 메서드는 값을 사용 하는 `GrayWorldDeviceWhiteBalanceGains` 속성 변경 내용을 적용 합니다.

흰색 밸런스 변경 수 있으려면 먼저 구성에 대 한 캡처 장치를 잠가야 합니다.

### <a name="manual-white-balance-example"></a>수동 흰색 균형 예제

현재 위치에서 일반 AV 캡처 설치 코드와는 `UIViewController` 응용 프로그램의 스토리 보드에 추가 하 고 다음과 같이 구성할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image18.png "UIViewController는 스토리 보드 응용 프로그램에 추가 하 고 여기 표시 된 대로 구성 수 있습니다.")](intro-to-manual-camera-controls-images/image18.png#lightbox)

뷰는 다음과 같은 주요 요소가 포함 되어 있습니다.

-  A `UIImageView` 비디오 피드에 표시할 합니다.
-  A `UISegmentedControl` 잠금을 포커스 모드에서 자동 변경 됩니다.
-  두 개의 `UISlider` 컨트롤을 표시 하 고 온도 Tint 업데이트 됩니다.
-  A `UIButton` 회색 카드 (회색 세계) 공간 샘플링 및 해당 값을 사용 하 여 흰색 균형을 설정 하는 데 사용 합니다.


와이어 접속 뷰 컨트롤러 수동 흰색 밸런스 컨트롤에 대 한 다음을 실행 합니다.


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
  
1. 다음과 같은 전용 변수를 추가 합니다.

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
  
1. 새 허용을 설정 하려면 다음 전용 메서드를 추가 온도 Tint 균형을 유지 합니다.

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
  
1. 재정의 `ViewDidLoad` 메서드를 다음 코드를 추가 합니다.

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
  
1. 재정의 `ViewDidAppear` 메서드 보기 로드 될 때 기록을 시작 하려면 다음 줄을 추가 합니다.

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
  
1. 변경 내용을 저장 된 하 고 응용 프로그램을 실행 합니다.
1. Auto 모드에서 카메라로 슬라이더 카메라 흰색 균형을 조정 하는 대로 자동으로 이동 합니다.

    [![](intro-to-manual-camera-controls-images/image19.png "카메라 흰색 균형을 조정 하는 대로 슬라이더는 자동으로 이동")](intro-to-manual-camera-controls-images/image19.png#lightbox)
1. 잠금 세그먼트 누르고 흰색 균형을 수동으로 조정 하려면 Temp 및 Tint 슬라이더를 끕니다.

    [![](intro-to-manual-camera-controls-images/image20.png "수동으로 흰색 균형을 조정 하려면 Temp 및 Tint 슬라이더를 끌어 옵니다.")](intro-to-manual-camera-controls-images/image20.png#lightbox)
1. 잠금 세그먼트를 선택한 상태와 물리적 회색 카드 카메라의 앞에 놓고 회색 세계에 흰색 균형을 조정 하려면 회색 카드 단추를 탭 합니다.

    [![](intro-to-manual-camera-controls-images/image21.png "회색 세계에 흰색 균형을 조정 하려면 회색 카드 단추를 누릅니다")](intro-to-manual-camera-controls-images/image21.png#lightbox)
1. 응용 프로그램을 중지 합니다.

위의 코드는 카메라 자동 모드에 있을 때 흰색 밸런스 설정이 모니터링 하거나 슬라이더를 사용 하 여 잠금 모드일 때 흰색 균형을 제어 하는 방법으로 나타났습니다.

## <a name="bracketed-capture"></a>대괄호로 묶은 캡처

대괄호로 묶은 캡처 위에 수동 카메라 컨트롤에서 설정에 기반 하 고 응용 프로그램을 여러 가지 다른 방법으로 시점에에서 캡처를 허용 합니다.

간단히 말하면 여전히 찍은 다양 한 설정 그림에서 그림의 캡처 대괄호로 묶습니다.

[![](intro-to-manual-camera-controls-images/image22.png "대괄호로 묶은 캡처 작동 방식")](intro-to-manual-camera-controls-images/image22.png#lightbox)

대괄호로 묶은 캡처를 사용 하 여 iOS 8에에서, 응용 프로그램 수 일련의 수동 카메라 컨트롤 미리 설정, 단일 명령을 실행 있고 각 수동 사전 설정에 대 한 일련의 이미지 반환 현재 장면.

### <a name="bracketed-capture-basics"></a>대괄호로 묶은 캡처 기본 사항

다시 캡처 대괄호로 묶은 그림 그림의 다양 한 설정으로 여전히 찍은입니다. 사용 가능한 캡처 대괄호로 묶은의 유형은 다음과 같습니다.

-  **자동으로 노출 대괄호** 모든 이미지는 다양 한 바이어스 크기가 있는 – 합니다.
-  **수동 노출 대괄호** – 다양 한 셔터 속도 (기간) 및 ISO 이미지가 모두 있는 양입니다.
-  **간단한 버스트 대괄호** – 일련의 연속적으로 빠르게 수행 하는 이미지 계속 합니다.


### <a name="new-bracketed-capture-controls-in-ios-8"></a>새 대괄호로 묶은 캡처 제어 iOS 8

모든 캡처 대괄호로 묶은 명령에서 구현 되는 `AVCaptureStillImageOutput` 클래스입니다. 사용 된 `CaptureStillImageBracket`설정의 지정 된 배열 사용 하 여 일련의 이미지를 가져올 메서드를 합니다.

두 가지 새 클래스가 설정을 처리를 구현 되었습니다.

-   `AVCaptureAutoExposureBracketedStillImageSettings` –에 하나의 속성만 `ExposureTargetBias`바이어스는 자동 노출 대괄호를 설정 하는 데 사용 되 합니다. 
-   `AVCaptureManual`  `ExposureBracketedStillImageSettings` -클래스에 두 개의 속성이 `ExposureDuration` 및 `ISO`, 수동 노출 대괄호 셔터 속도 ISO를 설정 하는 데 사용 합니다. 


### <a name="bracketed-capture-controls-dos-and-donts"></a>대괄호로 묶은 캡처 제어 관련 하 여 수행

#### <a name="dos"></a>관련

다음은 iOS 8에서에서 컨트롤을 대괄호로 묶은 캡처를 사용할 때 수행 해야 하는 것의 목록입니다.

-  호출 하 여 최악의 캡처 상황에 대 한 앱을 준비는 `PrepareToCaptureStillImageBracket` 메서드.
-  동일한 공유 풀에서 제공 하는 샘플 버퍼 것을 가정 합니다.
-  준비에 대 한 이전 호출에 의해 할당 된 메모리를 해제 하기 위해 호출 `PrepareToCaptureStillImageBracket` 다시 한 개체의 배열 보냅니다.


#### <a name="donts"></a>수행

다음은 iOS 8에서에서 컨트롤을 대괄호로 묶은 캡처를 사용할 때 실행 하지 않아야 하는 것의 목록입니다.

-  캡처 대괄호로 묶은 설정 형식에 단일 캡처에 모두 포함 되지 합니다.
-  요청 하지 않는 이상 `MaxBracketedCaptureStillImageCount` 에 단일 캡처의 이미지입니다.


### <a name="bracketed-capture-details"></a>대괄호로 묶은 캡처 세부 정보

다음 세부 정보를 고려해 야 캡처 대괄호로 묶은 iOS 8에서에서 작업할 때:

-  대괄호로 묶은 설정을 일시적으로 재정의 `AVCaptureDevice` 설정 합니다.
-  Flash 및 여전히 이미지 안정화 설정은 무시 됩니다.
-  모든 이미지는 동일한 출력 형식을 (jpeg, png, 등)를 사용 해야 합니다.
-  비디오 미리 보기 프레임을 삭제할 수 있습니다.
-  대괄호로 묶은 캡처는 iOS 8과 호환 되는 모든 장치에서 지원 됩니다.


이 정보에 주의 통해 iOS 8에에서 대괄호로 묶은 캡처를 사용 하는 예제를 살펴보겠습니다.

### <a name="bracket-capture-example"></a>대괄호 캡처 예제

현재 위치에서 일반 AV 캡처 설치 코드와는 `UIViewController` 응용 프로그램의 스토리 보드에 추가 하 고 다음과 같이 구성할 수 있습니다.

[![](intro-to-manual-camera-controls-images/image23.png "UIViewController는 스토리 보드 응용 프로그램에 추가 하 고 여기 표시 된 대로 구성 수 있습니다.")](intro-to-manual-camera-controls-images/image23.png#lightbox)

뷰는 다음과 같은 주요 요소가 포함 되어 있습니다.

-  A `UIImageView` 비디오 피드에 표시할 합니다.
-  세 가지 `UIImageViews` 는 캡처의 결과 표시 합니다.
-  A `UIScrollView` 비디오 피드 및 결과 뷰를 수용 하도록 합니다.
-  A `UIButton` 미리 설정 된 일부 설정을 사용 하 여 대괄호로 묶은 캡처를 수행 하는 데 사용 합니다.


와이어 접속 뷰 컨트롤러 캡처용으로 대괄호로 묶은 다음을 실행 합니다.


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
  
1. 다음과 같은 전용 변수를 추가 합니다.

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
  
1. 필요한 출력 이미지 뷰 작성 시키는 다음 전용 메서드를 추가 합니다.

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
  
1. 재정의 `ViewDidLoad` 메서드를 다음 코드를 추가 합니다.
    
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
    
  
1. 재정의 `ViewDidAppear` 메서드를 다음 코드를 추가 합니다.

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
    
1. 변경 내용을 저장 된 하 고 응용 프로그램을 실행 합니다.
1. 장면이 프레임 하 고 대괄호 캡처 단추를 탭 합니다.

    [![](intro-to-manual-camera-controls-images/image24.png "장면이 프레임 및 대괄호 캡처 단추를 누릅니다")](intro-to-manual-camera-controls-images/image24.png#lightbox)
1. 오른쪽에서 왼쪽으로 괄호로 묶입니다 캡처에 의해 수행 되는 세 가지 이미지를 보려면 살짝 밉니다.

    [![](intro-to-manual-camera-controls-images/image25.png "살짝 오른쪽에서 왼쪽으로 괄호로 묶입니다 캡처에 의해 수행 되는 세 가지 이미지를 보려면")](intro-to-manual-camera-controls-images/image25.png#lightbox)
1. 응용 프로그램을 중지 합니다.


위의 코드를 구성 하 고 iOS 8에는 자동 노출 대괄호로 묶은 캡처를 수행 하는 방법으로 나타났습니다.

## <a name="summary"></a>요약

이 문서에서 iOS 8에서 제공 하는 새 수동 카메라 컨트롤에 대 한 소개를 검사 했으며 결과 작동 하는 방법의 기본 사항을 설명 합니다. 수동 포커스, 수동 노출 및 수동 흰색 분산의 예를 제공 했습니다. 대괄호로 묶은 캡처 앞에서 설명한 수동 카메라 컨트롤을 사용 하 여 수행 하는 예로 제공 했습니다. 마지막으로,

## <a name="related-links"></a>관련 링크

- [ManualCameraControls (샘플)](https://developer.xamarin.com/samples/monotouch/ManualCameraControls)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
