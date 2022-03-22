# 安卓OpenGL ES技术实践

### 创建着色器程序
```
int vertexShader = loadShader(GLES20.GL_VERTEX_SHADER, vertexSource);
if (vertexShader == 0) {
    return 0;
}
int pixelShader = loadShader(GLES20.GL_FRAGMENT_SHADER, fragmentSource);
if (pixelShader == 0) {
    return 0;
}
int program = GLES20.glCreateProgram();
checkGlError("glCreateProgram");
if (program == 0) {
    Log.e(TAG, "Could not create program");
}
GLES20.glAttachShader(program, vertexShader);
checkGlError("glAttachShader");
GLES20.glAttachShader(program, pixelShader);
checkGlError("glAttachShader");
GLES20.glLinkProgram(program);
int[] linkStatus = new int[1];
GLES20.glGetProgramiv(program, GLES20.GL_LINK_STATUS, linkStatus, 0);
if (linkStatus[0] != GLES20.GL_TRUE) {
    Log.e(TAG, "Could not link program: ");
    Log.e(TAG, GLES20.glGetProgramInfoLog(program));
GLES20.glDeleteProgram(program);
    program = 0;
}
```

### 绘制纹理
```
// Select the program.
GLES20.glUseProgram(mProgramHandle);
GlUtil.checkGlError("glUseProgram");

// Set the texture.
GLES20.glActiveTexture(GLES20.GL_TEXTURE0);
GLES20.glBindTexture(mTextureTarget, textureId);

// Copy the model / view / projection matrix over.
GLES20.glUniformMatrix4fv(muMVPMatrixLoc, 1, false, mvpMatrix, 0);
GlUtil.checkGlError("glUniformMatrix4fv");

// Copy the texture transformation matrix over.
GLES20.glUniformMatrix4fv(muTexMatrixLoc, 1, false, texMatrix, 0);
GlUtil.checkGlError("glUniformMatrix4fv");

// Enable the "aPosition" vertex attribute.
GLES20.glEnableVertexAttribArray(maPositionLoc);
GlUtil.checkGlError("glEnableVertexAttribArray");

// Connect vertexBuffer to "aPosition".
GLES20.glVertexAttribPointer(maPositionLoc, coordsPerVertex,
    GLES20.GL_FLOAT, false, vertexStride, vertexBuffer);
GlUtil.checkGlError("glVertexAttribPointer");

// Enable the "aTextureCoord" vertex attribute.
GLES20.glEnableVertexAttribArray(maTextureCoordLoc);
GlUtil.checkGlError("glEnableVertexAttribArray");

// Connect texBuffer to "aTextureCoord".
GLES20.glVertexAttribPointer(maTextureCoordLoc, 2,
        GLES20.GL_FLOAT, false, texStride, texBuffer);
GlUtil.checkGlError("glVertexAttribPointer");

// Draw the rect.
GLES20.glDrawArrays(GLES20.GL_TRIANGLE_STRIP, firstVertex, vertexCount);
GlUtil.checkGlError("glDrawArrays");

// Done -- disable vertex array, texture, and program.
GLES20.glDisableVertexAttribArray(maPositionLoc);
GLES20.glDisableVertexAttribArray(maTextureCoordLoc);
GLES20.glBindTexture(mTextureTarget, 0);
GLES20.glUseProgram(0);
```

### 销毁
```
GLES20.glDeleteProgram(mProgramHandle);
```


### 参考
https://arm-software.github.io/opengl-es-sdk-for-android/introduction_to_shaders.html