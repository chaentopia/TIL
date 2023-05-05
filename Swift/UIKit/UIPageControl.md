# UIPageControl
<img width="69" alt="스크린샷 2023-05-05 오후 5 00 48" src="https://user-images.githubusercontent.com/109775321/236405832-c0c0dbcb-23c2-40d5-bb6a-866a0c3aa72f.png">
이런 것... UIPageControl이라고 합니다. 과제 진행하면서 UIPageControl에 무슨 속성이 있는지 궁금해져서 정의와 함께 찾아보고자 합니다.

공식문서에는 
> A control that displays a horizontal series of dots, each of which corresponds to a page in the app’s document or other data-model entity.
즉, 수평 점들로 이뤄져있는 컨트롤으로 각 점은 각 페이지에 해당된다는 의미를 갖고 있습니다.

```swift
@available(iOS 2.0, *)
@MainActor open class UIPageControl : UIControl {

    
    /// default is 0
    open var numberOfPages: Int

    
    /// default is 0. Value is pinned to 0..numberOfPages-1
    open var currentPage: Int

    
    /// hides the indicator if there is only one page, default is NO
    open var hidesForSinglePage: Bool

    
    /// The tint color for non-selected indicators. Default is nil.
    @available(iOS 6.0, *)
    open var pageIndicatorTintColor: UIColor?

    
    /// The tint color for the currently-selected indicators. Default is nil.
    @available(iOS 6.0, *)
    open var currentPageIndicatorTintColor: UIColor?

    
    /// The preferred background style. Default is UIPageControlBackgroundStyleAutomatic on iOS, and UIPageControlBackgroundStyleProminent on tvOS.
    @available(iOS 14.0, *)
    open var backgroundStyle: UIPageControl.BackgroundStyle

    
    /// The layout direction of the page indicators. The default value is \c UIPageControlDirectionNatural.
    @available(iOS 16.0, *)
    open var direction: UIPageControl.Direction

    
    /// The current interaction state for when the current page changes. Default is UIPageControlInteractionStateNone
    @available(iOS 14.0, *)
    open var interactionState: UIPageControl.InteractionState { get }

    
    /// Returns YES if the continuous interaction is enabled, NO otherwise. Default is YES.
    @available(iOS 14.0, *)
    open var allowsContinuousInteraction: Bool

    
    /// The preferred image for indicators. Symbol images are recommended. Default is nil.
    @available(iOS 14.0, *)
    open var preferredIndicatorImage: UIImage?

    
    /**
     * @abstract Returns the override indicator image for the specific page, nil if no override image was set.
     * @param page Must be in the range of 0..numberOfPages
     */
    @available(iOS 14.0, *)
    open func indicatorImage(forPage page: Int) -> UIImage?

    
    /**
     * @abstract Override the indicator image for a specific page. Symbol images are recommended.
     * @param image     The image for the indicator. Resets to the default if image is nil.
     * @param page      Must be in the range of 0..numberOfPages
     */
    @available(iOS 14.0, *)
    open func setIndicatorImage(_ image: UIImage?, forPage page: Int)

    
    /// The preferred image for the current page indicator. Symbol images are recommended. Default is nil.
    /// If this value is nil, then UIPageControl will use \c preferredPageIndicatorImage (or its per-page variant) as
    /// the indicator image.
    @available(iOS 16.0, *)
    open var preferredCurrentPageIndicatorImage: UIImage?

    
    /**
     * @abstract Returns the override current page indicator image for the specific page, nil if no override image was set.
     * @param page Must be in the range of 0..numberOfPages
     */
    @available(iOS 16.0, *)
    open func currentPageIndicatorImage(forPage page: Int) -> UIImage?

    
    /**
     * @abstract Override the current page indicator image for a specific page. Symbol images are recommended.
     * @param image     The image for the indicator. Resets to the default if image is nil.
     * @param page      Must be in the range of 0..numberOfPages
     */
    @available(iOS 16.0, *)
    open func setCurrentPageIndicatorImage(_ image: UIImage?, forPage page: Int)

    
    /// Returns the minimum size required to display indicators for the given page count. Can be used to size the control if the page count could change.
    open func size(forNumberOfPages pageCount: Int) -> CGSize

    
    /// if set, tapping to a new page won't update the currently displayed page until -updateCurrentPageDisplay is called. default is NO
    @available(iOS, introduced: 2.0, deprecated: 14.0, message: "defersCurrentPageDisplay no longer does anything reasonable with the new interaction mode.")
    open var defersCurrentPageDisplay: Bool

    
    /// update page display to match the currentPage. ignored if defersCurrentPageDisplay is NO. setting the page value directly will update immediately
    @available(iOS, introduced: 2.0, deprecated: 14.0, message: "updateCurrentPageDisplay no longer does anything reasonable with the new interaction mode.")
    open func updateCurrentPageDisplay()
}
```
정의를 찾아보니까 굉장히 많은 속성들을 갖고 있는데, 여기서 몇 가지 속성들만 확인해보자면
`var numberOfPages: Int`는 page의 개수, 즉 dots의 개수들이 됩니다. 정적인 경우 직접 정수를 입력해주면 되지만 서버에서 받아오거나 동적이 될 경우 count로 설정해줍니다. 기본값은 0입니다.
`var currentPage: Int`는 현재 페이지로, 기본값은 마찬가지로 0입니다.
`var hidesForSinglePage: Bool`는 page가 하나라면 이 UIPageControl을 숨길 것이냐고 묻는 속성으로, 기본값은 NO입니다.
`var pageIndicatorTintColor: UIColor?`는 선택되지 않은 점의 색상이고
`var currentPageIndicatorTintColor: UIColor?`는 선택된 점의 색상입니다.
기본적으로 해당 속성들을 사용하게 됩니다.

```swift
private let pageControl = UIPageControl().then {
        $0.numberOfPages = 6
        $0.currentPage = 0
        $0.pageIndicatorTintColor = .gray3
        $0.currentPageIndicatorTintColor = .white
        $0.transform = CGAffineTransform(scaleX: 0.7 , y: 0.7)
    }
```
저는 이렇게 사용해서 아래와 같이 나타내주었습니다!
![Simulator Screenshot - iPhone 14 Pro - 2023-05-05 at 17 11 24](https://user-images.githubusercontent.com/109775321/236407935-d59c27be-a760-4b25-98aa-0a5c0a6ea2f4.png)
