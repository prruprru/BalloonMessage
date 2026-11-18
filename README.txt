BalloonMessage.dll - VFP9 32 bits

Archivos:
- BalloonMessage.cpp
- BalloonMessage.def

Configuración:
- Visual Studio 2022
- Release | Win32
- Dynamic Library (.dll)
- Platform Toolset: v143
- C++ Language Standard: C++17 (o el estándar predeterminado del proyecto)
- Linker / Module Definition File: BalloonMessage.def

La DLL usa GDI+, User32 y Common Controls (ToolTip nativo de Windows).

Exportaciones:
BM_ShowEx(HWND owner, int x, int y, LPCSTR text, int timeoutMs,
          COLORREF backColor, int maxWidth)
BM_Hide()
BM_Destroy()
BM_IsVisible()
BM_Version()

Parámetros:
- owner: HWND del formulario/control propietario. Puede ser 0.
- x, y: coordenadas cliente de owner. La DLL las convierte a pantalla.
- text: texto ANSI (página de códigos del sistema).
- timeoutMs: > 0 = autocierre en milisegundos; 0 = permanencia manual.
- backColor: COLORREF; en VFP se pasa directamente el resultado de RGB().
- maxWidth: ancho máximo total en píxeles, incluida la sombra. Si es <= 0, usa 360.

El ToolTip aparece únicamente si el texto excede el ancho disponible. El texto visible
se trunca con puntos suspensivos y el ToolTip muestra el texto completo.

VFP9 (32 bits):
DECLARE INTEGER BM_ShowEx IN BalloonMessage.dll ;
    INTEGER hOwner, INTEGER nX, INTEGER nY, ;
    STRING cText, INTEGER nTimeout, ;
    INTEGER nBackColor, INTEGER nMaxWidth

DECLARE INTEGER BM_Hide IN BalloonMessage.dll
DECLARE INTEGER BM_Destroy IN BalloonMessage.dll
DECLARE INTEGER BM_IsVisible IN BalloonMessage.dll
DECLARE INTEGER BM_Version IN BalloonMessage.dll

Ejemplo:
lnResultado = BM_ShowEx(Thisform.HWnd, 20, 20, ;
    "Texto de notificación", 3000, RGB(255,245,190), 360)

Autocierre:
- timeoutMs = 3000: se oculta aproximadamente a los 3 segundos.
- timeoutMs = 0: no se oculta por tiempo; VFP debe llamar BM_Hide().

Notas:
- La ventana no toma el foco.
- Para que el ToolTip pueda detectar el puntero, el globo recibe el hit-test del mouse.
  Por ello, a diferencia del Balloon anterior con HTTRANSPARENT, el globo no deja pasar
  los clics que se realizan exactamente encima de él. No activa la ventana.
- El temporizador se procesa mediante los mensajes de la ventana y requiere que el hilo
  de VFP siga procesando mensajes.
- Compilar como Win32; esta DLL es para VFP9 de 32 bits.
