# 멀티테스킹 예시
## 출처
https://blog.naver.com/wheelcar/221986293094
## 구현
```c++
unsigned int ThreadProcess(LPVOID pParam)       // 쓰레드로 처리할 일반함수
{
	CDC dc;
	HDC hDC = ::GetDC(HWND(pParam));   //pParam을 통해 this->m_hWnd 매개변수가 전달됨
	//GetDC함수는 매개변수의 dc를 가져옮
	//원형 CDC* GetDC(LPCRECT lprcRect = NULL, DWORD dwFlags = OLEDC_PAINTBKGND);
	// lprcRect : 창없는 작업영역의 클라이언트 좌표에서 사각형에 대한 포인터 NULL은 전체객체의 범위
	// dwFlags : OLEDC_PAINTBKGND, OLEDC_NODRAW, LOEDC_OFFSCREEN
	dc.Attach(hDC);   // 새로만든 핸들, 래퍼객체와 Windows 객체에 대해 핸들을 지정하며 dc객체를 뷰클래스의 작업영역 핸들임

	CPen Mypen, * pOldpen;
	int i, j;
	dc.SelectStockObject(NULL_BRUSH);
	for (j = 0; j < 255; j++)      // 255값을 증가시키면 원을 더 오래 그린다.
	{
		for (i = 0; i < 255; i++)
		{
			Mypen.CreatePen(PS_SOLID, 2, i << (j % 3 * 8));
			pOldpen = dc.SelectObject(&Mypen);
			dc.Ellipse(300 - i, 300 - i, 300 + i, 300 + i);
			dc.SelectObject(&pOldpen);
			Mypen.DeleteObject();
			Sleep(25);
		}
	}
	dc.Detach();     // dc를 해제함. 함수가 종료되기 전에 dc를 m_hWnd에서 분리해야 함.
	::ReleaseDC(HWND(pParam), hDC); // int Release(CDC* pDC); 원형으로 DC를 해제해 줌.
	return 0;
}

void CMFCApplication3View::OnLButtonDown(UINT nFlags, CPoint point)
{
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.
	AfxBeginThread(ThreadProcess, this->m_hWnd);
	CView::OnLButtonDown(nFlags, point);
}


void CMFCApplication3View::OnRButtonDown(UINT nFlags, CPoint point)
{
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.
	CClientDC dc(this);
	dc.TextOutW(point.x, point.y, L"User Message");


	CView::OnRButtonDown(nFlags, point);
}
```
* 마우스를 좌클릭하여 쓰레드를 하나 불러서 원을 계속 만들도록 무한반복 시키는 와중에 우클릭을 해도 작업이 끊어지지 않음
