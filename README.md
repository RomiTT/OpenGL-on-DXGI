# OpenGL-on-DXGI
How to use WGL_NV_DX_interop2 to use OpenGL in a DXGI window

## NVIDIA

Tested on GTX 970.

On NVIDIA, this example works fine with a `DXGI_SWAP_EFFECT_DISCARD` swap chain. It seems NVIDIA drivers are currently not updated for Windows 10 swap chains (like `DXGI_SWAP_EFFECT_FLIP_DISCARD`).

Currently, `FLIP_DISCARD` seems to only work for the swap chain's first buffer (strangely), resulting in flickering. The screen is black for all frames that aren't using the first buffer of the swap chain.

Also, there are some exceptions being thrown when the swap chain buffer is registered as an OpenGL resource, which you can see logged in the debug output window. It's spammy, but seems you can ignore them safely.

Also, I'm getting errors in the framebuffer's status. Not sure why yet. The error is different between the first frame of rendering and subsequent frames.

## Intel

Intel recently started supporting `WGL_NV_DX_interop2`, but I haven't figured out how to get the bleeding edge drivers to use it.

## AMD

Haven't tested on AMD yet. Planning to grab an AMD GPU soon-ish, and will test it then.

## Conclusions

This extension's support doesn't yet match the capabilities of using DXGI with plain D3D. Since most of the bugginess comes from trying to access swap chain buffers from OpenGL, you might be able to get away with using this extension by not trying to wrap the swap chain buffers and instead just doing a copy at the end of your frame. Unfortunately, it's easy to introduce extra presentation latency that way.
