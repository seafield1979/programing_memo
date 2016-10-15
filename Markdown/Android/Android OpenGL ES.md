#OpenGL
OpenGL ES 1系で3D描画を行う。

リンク

[独学で 1 ヶ月間 OpenGL を学んで得た基礎知識のまとめ ~ 2D 編 ~](http://tkengo.github.io/blog/2014/12/20/opengl-es-2-2d-knowledge-0/)
[OpenGL による 3Dグラフィックス](http://android.keicode.com/basics/opengl.php)


OpenGL ES1を使って最低限の描画を行うには以下の実装が必要。

* GLSurfaceViewのサブクラス
* Rendererのサブクラス

```java
- MyGLView.java -

import android.content.Context;
import android.opengl.GLSurfaceView;

public class MyGLView extends GLSurfaceView {

    MyRenderer myRenderer;

    public MyGLView(Context context) {
        super(context);
        myRenderer = new MyRenderer();
        setRenderer(myRenderer);
    }
}
```

```java
- MyRenderer.java -

import javax.microedition.khronos.egl.EGLConfig;
import javax.microedition.khronos.opengles.GL10;

import android.opengl.GLSurfaceView.Renderer;
import android.opengl.GLU;

public class MyRenderer implements Renderer {

    MyCube cube = new MyCube();
    int mCount = 0;

    // 描画処理
    @Override
    public void onDrawFrame(GL10 gl) {
        gl.glClear(GL10.GL_COLOR_BUFFER_BIT
                | GL10.GL_DEPTH_BUFFER_BIT);

        gl.glMatrixMode(GL10.GL_MODELVIEW);
        gl.glLoadIdentity();
        gl.glTranslatef(0, 0, -3f);

        // 回転
        gl.glRotatef(30f, 0, 1, 0);

        cube.draw(gl);
    }

    // 生成時
    @Override
    public void onSurfaceCreated(GL10 gl, EGLConfig config) {
        gl.glEnable(GL10.GL_DEPTH_TEST);
        gl.glDepthFunc(GL10.GL_LEQUAL);

        // 光源
        gl.glEnable(GL10.GL_LIGHTING);
        gl.glEnable(GL10.GL_LIGHT0);

    }

    // サイズ変更(端末の向きが変更された場合など)
    @Override
    public void onSurfaceChanged(GL10 gl, int width, int height) {
        gl.glViewport(0, 0, width, height);

        gl.glMatrixMode(GL10.GL_PROJECTION);
        gl.glLoadIdentity();
        GLU.gluPerspective(gl, 45f,(float) width / height, 1f, 50f);
    }
}
```

```java
- MyCube.java -
import java.nio.ByteBuffer;
import java.nio.ByteOrder;
import java.nio.FloatBuffer;

import javax.microedition.khronos.opengles.GL10;

public class MyCube {

    private final FloatBuffer mVertexBuffer;

    public MyCube(){
        float vertices[] = {
            // 前
            -0.5f, -0.5f, 0.5f,
            0.5f, -0.5f, 0.5f,
            -0.5f, 0.5f, 0.5f,
            0.5f, 0.5f, 0.5f,

            // 後
            -0.5f, -0.5f, -0.5f,
            0.5f, -0.5f, -0.5f,
            -0.5f, 0.5f, -0.5f,
            0.5f, 0.5f, -0.5f,

            // 左
            -0.5f, -0.5f, 0.5f,
            -0.5f, -0.5f, -0.5f,
            -0.5f, 0.5f, 0.5f,
            -0.5f, 0.5f, -0.5f,

            // 右
            0.5f, -0.5f, 0.5f,
            0.5f, -0.5f, -0.5f,
            0.5f, 0.5f, 0.5f,
            0.5f, 0.5f, -0.5f,

            // 上
            -0.5f, 0.5f, 0.5f,
            0.5f, 0.5f, 0.5f,
            -0.5f, 0.5f, -0.5f,
            0.5f, 0.5f, -0.5f,

            // 底
            -0.5f, -0.5f, 0.5f,
            0.5f, -0.5f, 0.5f,
            -0.5f, -0.5f, -0.5f,
            0.5f, -0.5f, -0.5f
        };

        ByteBuffer vbb =
                ByteBuffer.allocateDirect(vertices.length * 4);
        vbb.order(ByteOrder.nativeOrder());
        mVertexBuffer = vbb.asFloatBuffer();
        mVertexBuffer.put(vertices);
        mVertexBuffer.position(0);

    }

    public void draw(GL10 gl){

        gl.glEnableClientState(GL10.GL_VERTEX_ARRAY);
        gl.glVertexPointer(3, GL10.GL_FLOAT, 0, mVertexBuffer);

        // Front
        gl.glNormal3f(0, 0, 1.0f);
        gl.glDrawArrays(GL10.GL_TRIANGLE_STRIP, 0, 4);

        // Back
        gl.glNormal3f(0, 0, -1.0f);
        gl.glDrawArrays(GL10.GL_TRIANGLE_STRIP, 4, 4);

        // Left
        gl.glNormal3f(-1.0f, 0, 0);
        gl.glDrawArrays(GL10.GL_TRIANGLE_STRIP, 8, 4);

        // Right
        gl.glNormal3f(1.0f, 0, 0);
        gl.glDrawArrays(GL10.GL_TRIANGLE_STRIP, 12, 4);

        // Top
        gl.glNormal3f(0, 1.0f, 0);
        gl.glDrawArrays(GL10.GL_TRIANGLE_STRIP, 16, 4);

        // Right
        gl.glNormal3f(0, -1.0f, 0);
        gl.glDrawArrays(GL10.GL_TRIANGLE_STRIP, 20, 4);

    }
}
```

```java
- MainActivity.java -

public class MainActivity extends AppCompatActivity {

    MyGLView myGLView;
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        myGLView = new MyGLView(this);
        setContentView(myGLView);
    }

    @Override
    protected void onResume(){
        super.onResume();
        myGLView.onResume();
    }

    @Override
    protected void onPause(){
        super.onPause();
        myGLView.onPause();
    }
}
```