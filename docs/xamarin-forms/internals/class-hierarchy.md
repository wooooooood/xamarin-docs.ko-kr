---
제목: " Xamarin.Forms 컨트롤 클래스 계층 구조" 설명: "개발자는 응용 프로그램의 사용자 인터페이스를 만드는 데 사용 되는 형식의 계층 구조에 대해 잘 알고 있어야 Xamarin.Forms 합니다."
assetid: C89E6B98-464D-4BBE-BF11-13A5FCBBF420: xamarin-forms author: davidbritch: dabritch:: 01/07/2020-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="xamarinforms-controls-class-hierarchy"></a>Xamarin.FormsControls 클래스 계층 구조

Xamarin.Forms는 여러 네임 스페이스에 대해 수백 개의 형식으로 구성 됩니다. 개발자는 Xamarin.Forms 네임 스페이스에 상주 하는 응용 프로그램의 사용자 인터페이스를 만드는 데 사용 되는 형식의 계층 구조에 대해 가장 잘 알고 있어야 합니다 `Xamarin.Forms` .

이러한 형식은 페이지, 레이아웃, 보기 및 셀로 나눌 수 있습니다. Xamarin.Forms페이지는 일반적으로 전체 화면을 차지 하며 모든 페이지 형식은 클래스에서 파생 [`Page`](xref:Xamarin.Forms.Page) 됩니다. 페이지는 일반적으로 레이아웃을 포함 하 고 모든 레이아웃 형식은 클래스에서 파생 [`Layout`](xref:Xamarin.Forms.Layout) 됩니다. 레이아웃은 일반적으로 뷰 및 다른 레이아웃을 포함 하며 모든 뷰 형식은 궁극적으로 클래스에서 파생 [`View`](xref:Xamarin.Forms.View) 됩니다. 마지막으로 셀은 및 컨트롤의 표시 데이터에 사용 되는 특수 [`TableView`](xref:Xamarin.Forms.TableView) 컨트롤 [`ListView`](xref:Xamarin.Forms.ListView) 입니다. 페이지, 레이아웃, 보기 및 셀은 모두 궁극적으로 클래스에서 파생 됩니다 [`Element`](xref:Xamarin.Forms.Element) .

다음 클래스 다이어그램에서는에서 사용자 인터페이스를 작성 하는 데 일반적으로 사용 되는 형식의 계층 구조를 보여 줍니다 Xamarin.Forms .

[![Xamarin.FormsControls 클래스 다이어그램](class-hierarchy-images/class-diagram.png "[! OP. NO-LOC (Xamarin.ios)] controls 클래스 다이어그램")](class-hierarchy-images/class-diagram-large.png#lightbox "[! OP. NO-LOC (Xamarin.ios)] controls 클래스 다이어그램")

> [!NOTE]
> 클래스 다이어그램의 고해상도 버전은 [여기](class-hierarchy-images/class-diagram-high-resolution.png)에서 다운로드할 수 있습니다. 그러나 다이어그램은 단일 셸 유형만 표시 합니다.

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms컨트롤 참조](~/xamarin-forms/user-interface/controls/index.md)
