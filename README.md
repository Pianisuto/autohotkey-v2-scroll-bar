# autohotkey-v2-scroll-bar

This class is based on research of "winuser.h" and some posts from the autohotkey forums, particularly the post located at:
https://www.autohotkey.com/board/topic/26033-scrollable-gui-proof-of-concept/#entry168174

The class provides functionality to create a scrollable GUI with the use of a scrollbar.

Example:

``` autohotkey
M := Gui('+Resize')
Loop 15 {
    M.Add("Button", "x5 y" 10 + ((A_Index - 1) * 100) " w800 h80", A_Index)
}
SB := ScrollBar(M, 120, 200)
M.Show('w500 h200')
M.OnEvent "Close", (*) => ExitApp()
M.OnEvent "Escape", (*) => ExitApp()

#HotIf WinActive(M.Hwnd)
    WheelUp::
    WheelDown::
    +WheelUp::
    +WheelDown:: {
        SB.ScrollMsg(InStr(A_ThisHotkey,"Down") ? 1 : 0, 0, GetKeyState("Shift") ? 0x114 : 0x115, M.Hwnd)
        return
    }
#HotIf
