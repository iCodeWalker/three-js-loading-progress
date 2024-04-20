# Three.js loading process

1. We will add a simple loader so that the scene appears nicely when everything is ready.
2. We are going to use a mix of WebGL and HTML/CSS for the loader.

3. We will add a black overlay that fades out when everything is ready.
   There are many solutions.

   1. Animate the <canvas> in CSS.
   2. Animate a <div> above the <canvas> in CSS.
   3. Animate a black rectangle in front of the camera.

4. To put a plane infront of the camera, we can add it to the camera object and move it a bit forward.
   Instead we are going to put the plane on the scene and position it's vertices using the vertex shader.

   Create an overlay.
   const overlayGeometry = new THREE.PlaneGeometry(1, 1, 1, 1);
   const overlayMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 });
   const overlay = new THREE.Mesh(overlayGeometry, overlayMaterial);

   scene.add(overlay);

5. Replace MeshBasicMaterial with ShaderMaterial.
   const overlayMaterial = new THREE.ShaderMaterial({
   vertexShader: `   void main()
{
    gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
}`,
   fragmentShader: `   void main()
{
    gl_FragColor = vec4(1.0, 0.0, 0.0, 1.0)
}`,
   });

6. The coordinates of the plane's vertices go from -0.5 to +0.5 because our plane has a size of 1.

7. To get the bigger rectangle, we need the coordinates to go from -1 to +1. Double the size of PlaneGeometry.
   const overlayGeometry = new THREE.PlaneGeometry(2, 2, 1, 1);

8. Change the plane color to black.  
   gl_FragColor = vec4(0.0, 0.0, 0.0, 1.0);

9. Add an uniform to control aplha(uAlpha);
   Retrieve uAlpha inside fragmentShader, and use it.
