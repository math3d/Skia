[English](https://github.com/math3d/Skia/blob/master/README_en_GB.md) | [简体中文](https://github.com/math3d/Skia/blob/master/README.md)

## 基于网页的Skia测试方法

测试步骤：

 1. 打开网页https://fiddle.skia.org/, 点击“Options”，Width输入512，Height输入512。"Optional
    source image"选择第三个图片；
 2. 程序清单里面的代码替换； 
 3. 点击Run运行程序；


## 主要测试程序

[程序清单 13 1 SkCanvas::drawImage例子](https://fiddle.skia.org/c/7fefce4b170126fe9eb9a94050138719)
```
void draw(SkCanvas* canvas) {
  SkPaint paint;
  canvas->drawImage(image, 0, 0, &paint);
  return;
}
```

[程序清单 13 2 SkCanvas::drawRect例子](https://fiddle.skia.org/c/23594f680d4e03468a9fb28ff02174f3)
```
void draw(SkCanvas* canvas) {
  SkPaint paint;
  SkRect rect = {0, 0, 512, 512};
  canvas->drawRect(rect, paint);
}
```
[程序清单 13 3 SkCanvas::drawImageRect例子](https://fiddle.skia.org/c/01d1d5d10dbc47022146e025405afc9a)
```
void draw(SkCanvas* canvas) {
  SkPaint paint;
  SkRect rect = SkRect::MakeXYWH(0, 0, 512, 512);
  canvas->drawImageRect(image, SkRect{0, 0, 512, 512}, rect, &paint,
    SkCanvas::kFast_SrcRectConstraint);
  return;
}
```
[程序清单 13 4 设置输出背景色](https://fiddle.skia.org/c/2cdc29029cf47b18d25a7377a6a2991b)

```
void draw(SkCanvas* canvas) {
  SkPaint paint;
  paint.setColor(SK_ColorLTGRAY);
  canvas->drawPaint(paint); 
  return;
}
```
[程序清单 13 6 SkCanvas::drawRect的默认场景](https://fiddle.skia.org/c/47c440a2ab3e043659a22845f0b7bdb8)
```
void draw(SkCanvas* canvas) {
  SkPaint paint;
  paint.setColor(SK_ColorGRAY);
  SkRect rect = SkRect::MakeXYWH(0, 0, 512, 512);
  canvas->drawRect(rect, paint);
  return;
}
```
[程序清单 13 7 使用归一化的输入](https://fiddle.skia.org/c/cf60a88d26a8f3cde7d2d0390bb36eda)
```
void draw(SkCanvas* canvas) {
  SkPaint paint;
  paint.setColor(SK_ColorGRAY);
  // 输入归一化的矩形。
  SkRect rect = SkRect::MakeXYWH(0.0, 0.0, 1.0, 1.0);
  SkMatrix matrix;
  matrix.setTranslate(0.0, 0.0);
  matrix.preScale(512.0, 512.0);
  // 设置模型变换矩阵。
  canvas->setMatrix(matrix);
  canvas->drawRect(rect, paint);
  return;
}
```

[程序清单 13 8 绘制图形的偏移](https://fiddle.skia.org/c/9ab92018fb8c8d5ef622efb6652f9a35)
```
void draw(SkCanvas* canvas) {
  SkPaint paint;
  paint.setColor(SK_ColorGRAY);
  SkRect rect = SkRect::MakeXYWH(-0.25, -0.25, 1.0, 1.0);
  SkMatrix matrix;
  matrix.setTranslate(128.0, 128.0);
  matrix.preScale(512.0, 512.0);
  canvas->setMatrix(matrix);
  canvas->drawRect(rect, paint);
  return;
}
```

[程序清单 13 9 调整输入矩形可以让输出图形产生偏移](https://fiddle.skia.org/c/0bff2ce56c00fa87c1348df7e08ae5a4)
```
void draw(SkCanvas* canvas) {
  SkPaint paint;
  // 背景绘制为淡灰色。
  paint.setColor(SK_ColorLTGRAY);
  canvas->drawPaint(paint); 
  paint.setColor(SK_ColorWHITE);
  SkRect rect = SkRect::MakeXYWH(0.5, 0.5, 0.5, 0.5);
  SkMatrix matrix;
  matrix.setTranslate(0.0, 0.0);
  matrix.preScale(512.0, 512.0);
  canvas->setMatrix(matrix);
  canvas->drawRect(rect, paint);
  return;
}
```

[程序清单 13 10 图文混排](https://fiddle.skia.org/c/f30a7f950a536659bde1b878321a8686)
```
void RectToTransform(SkMatrix* quad_matrix, const SkRect& quad_rect) {
  quad_matrix->setTranslate(quad_rect.x(), quad_rect.y());
  quad_matrix->preScale(quad_rect.width(), quad_rect.height());
}
void draw(SkCanvas* canvas) {
  SkPaint paint;
  SkRect rect = SkRect::MakeXYWH(0.0, 0.0, 1.0, 0.9);
  SkMatrix matrix;
  matrix.reset();
  SkRect real_rect = SkRect::MakeXYWH(0.0, 0.0, 512.0, 512.0);
  RectToTransform(&matrix, real_rect);
  // 绘制图片的时候使用的模型矩阵将输入的坐标变成了归一化的。
  canvas->setMatrix(matrix);
  SkRect texture_rect = SkRect::MakeXYWH(0, 0, 512, 512);
  canvas->drawImageRect(image, texture_rect, rect, &paint,
      SkCanvas::kFast_SrcRectConstraint);
  // 绘制文字使用不同的模型变换矩阵。
  SkMatrix matrix_text;
  matrix_text.reset();
  SkRect real_rect_text = SkRect::MakeXYWH(0.0, 0.0, 2.0, 2.0);
  RectToTransform(&matrix_text, real_rect_text);
  canvas->setMatrix(matrix_text);
  paint.setTextSize(10);
  paint.setColor(SK_ColorGRAY);
  SkString str("HelloWorld");
  canvas->drawString(str, 100, 245, paint);
  return;
}
```

[程序清单 13 11 图片被裁减](https://fiddle.skia.org/c/89681768c2f88106fb8c473d4612cbef)
```
void draw(SkCanvas* canvas) {
    SkPaint paint;
    SkRect rect = SkRect::MakeXYWH(0, 0, 512, 512);
    SkRect texture_rect = SkRect::MakeXYWH(0, 0, 384, 512);
    canvas->drawImageRect(image, texture_rect, rect, &paint,
        SkCanvas::kFast_SrcRectConstraint);
    return;
}
```
[程序清单 13 12 图片被裁减例子2](https://fiddle.skia.org/c/d3656a7d82fb4863efbc4682abba98e7)
```
void draw(SkCanvas* canvas) {
    SkPaint paint;
    SkRect rect = SkRect::MakeXYWH(0, 0, 512, 512);
    SkRect texture_rect = SkRect::MakeXYWH(0, 0, 256, 256);
    canvas->drawImageRect(image, texture_rect, rect, &paint,
        SkCanvas::kFast_SrcRectConstraint);
    return;
}
```

[程序清单 13 13 纹理坐标不受模型变换的影响](https://fiddle.skia.org/c/93332b706b004fde174bdae566570357)
```
void RectToTransform(SkMatrix* quad_matrix, const SkRect& quad_rect) {
  quad_matrix->setTranslate(quad_rect.x(), quad_rect.y());
  quad_matrix->preScale(quad_rect.width(), quad_rect.height());
}
void draw(SkCanvas* canvas) {
  SkPaint paint;
  SkRect rect = SkRect::MakeXYWH(0.0, 0.0, 1.0, 1.0);
  SkMatrix matrix;
  matrix.reset();
  SkRect real_rect = SkRect::MakeXYWH(0.0, 0.0, 512.0, 512.0);
  RectToTransform(&matrix, real_rect);
  canvas->setMatrix(matrix);
  SkRect texture_rect = SkRect::MakeXYWH(0, 0, 512, 512);
  canvas->drawImageRect(image, texture_rect, rect, &paint,
      SkCanvas::kFast_SrcRectConstraint);
  return;
}
```

[程序清单 13 17 窗口原点位于窗口左上角](https://fiddle.skia.org/c/a1358ce304347e09f660ee8d3f99eca1)
```
// 正交投影矩阵。
static SkMatrix44 OrthoProjectionMatrix(float left,
                                        float right,
                                        float bottom,
                                        float top) {
  float delta_x = right - left;
  float delta_y = top - bottom;
  SkMatrix44 proj;
  if (!delta_x || !delta_y)
    return proj;
  proj.set(0, 0, 2.0f / delta_x);
  proj.set(0, 3, -(right + left) / delta_x);
  proj.set(1, 1, 2.0f / delta_y);
  proj.set(1, 3, -(top + bottom) / delta_y);
  // 由于没有使用深度信息，所以z分量的系数为0。
  proj.set(2, 2, 0);
  return proj;
}
// 窗口映射矩阵。
static SkMatrix44 WindowMatrix(SkRect window) {
  SkMatrix44 canvas;
  // 将[(0, 0), (1, 1)]映射到窗口坐标。
  canvas.preTranslate(window.x(), window.y(), 0);
  canvas.preScale(window.width(), window.height(), 0);
  // 从[(−1, −1), (1, 1)] 映射到[(0, 0), (1, 1)]。
  canvas.preTranslate(0.5, 0.5, 0.5);
  canvas.preScale(0.5, 0.5, 0.5);
  return canvas;
}
/* RectToTransform将窗口的实际大小转换为一个模型变换矩阵。这个变换的特点是：用户输入的顶点坐标的有效范围是[(0, 0), (1, 1)]。譬如下面的代码将绘制一个填充整个窗口的矩形：
  SkRect rect = {0, 0, 1, 1};
  canvas->drawRect(rect, paint);
*/ 
void RectToTransform(SkMatrix44* quad_matrix, const SkRect& quad_rect) {
  quad_matrix->preTranslate(quad_rect.x(), quad_rect.y(), 0);
  quad_matrix->preScale(quad_rect.width(), quad_rect.height(), 1);
}
void TransformToFlattenedSkMatrix(const SkMatrix44& transform,
                                  SkMatrix* flattened) {
  // 删除第三行和第三列，将 4×4 矩阵变成3×3 矩阵。具体
  // 参考公式 13 1 SkMatrix和SkMatrix44的转换。
  flattened->set(0, SkMScalarToScalar(transform.get(0, 0)));
  flattened->set(1, SkMScalarToScalar(transform.get(0, 1)));
  flattened->set(2, SkMScalarToScalar(transform.get(0, 3)));
  flattened->set(3, SkMScalarToScalar(transform.get(1, 0)));
  flattened->set(4, SkMScalarToScalar(transform.get(1, 1)));
  flattened->set(5, SkMScalarToScalar(transform.get(1, 3)));
  flattened->set(6, SkMScalarToScalar(transform.get(3, 0)));
  flattened->set(7, SkMScalarToScalar(transform.get(3, 1)));
  flattened->set(8, SkMScalarToScalar(transform.get(3, 3)));
}
void draw(SkCanvas* canvas) {
  SkPaint paint;
  SkMatrix44 obj_matrix;
  obj_matrix.reset();
  SkRect real_rect = SkRect::MakeXYWH(0.0, 0.0, 512.0, 512.0);
  RectToTransform(&obj_matrix, real_rect);
  SkMatrix44 window_matrix;
  window_matrix.reset();
  window_matrix = WindowMatrix(real_rect);
  SkMatrix44 proj_matrix;
  proj_matrix.reset();
  // 使用正交投影矩阵。
  proj_matrix =
      OrthoProjectionMatrix(real_rect.x(), real_rect.x() + real_rect.width(),
                            real_rect.y(), real_rect.y() + real_rect.height());
  SkMatrix44 mvp_matrix;
  mvp_matrix.reset();
  // 物体坐标经过模型变换、投影变换、窗口变换得到窗口坐标。
  mvp_matrix = window_matrix * proj_matrix * obj_matrix;
  SkMatrix sk_device_matrix;
  TransformToFlattenedSkMatrix(mvp_matrix, &sk_device_matrix);
  canvas->setMatrix(sk_device_matrix);
  canvas->drawCircle(0.0,0.0,0.1,paint);
  return;
}
```

[程序清单 13 19 Skia实现的图像边缘检测](https://fiddle.skia.org/c/d2d036ec05e86e76341a9fad2e3fc44e)
```
#include "include/effects/SkMatrixConvolutionImageFilter.h"
void draw(SkCanvas* canvas) {
  SkScalar kernel[9] = {
      SkIntToScalar(1), SkIntToScalar(2), SkIntToScalar(1),
      SkIntToScalar(0), SkIntToScalar(0),  SkIntToScalar(0),
      SkIntToScalar(-1), SkIntToScalar(-2), SkIntToScalar(-1),
  };
  SkISize kernelSize = SkISize::Make(3, 3);
  SkScalar gain = 1.0f, bias = SkIntToScalar(0);
  SkIPoint kernelOffset = SkIPoint::Make(1, 1);
  auto tileMode = SkMatrixConvolutionImageFilter::kClamp_TileMode;
  bool convolveAlpha = false;
  sk_sp<SkImageFilter> convolve(SkMatrixConvolutionImageFilter::Make(
      kernelSize, kernel, gain, bias, kernelOffset, tileMode, convolveAlpha,
      nullptr));
  SkPaint paint;
  SkIRect subset = image->bounds();
  SkIRect clipBounds = image->bounds();
  SkIRect outSubset;
  SkIPoint offset;
  sk_sp<SkImage> filtered(image->makeWithFilter(
      convolve.get(), subset, clipBounds, &outSubset, &offset));
  paint.setAntiAlias(true);
  paint.setStyle(SkPaint::kStroke_Style);
  canvas->drawImage(filtered, 0, 0);
}
```
