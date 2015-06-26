<img src="http://myleia.com/github/LeiaGitHub.png"/>

Building off of our [previous example](https://github.com/LeiaInc/LeiaBasicAnimation), lets see what it takes to add some cool visual effects to our simple animated four shape scene. In this case, lets add some shadows.

We'll be starting with our previous HTML code that looks like this:

    <!DOCTYPE html>
    <head>
        <meta charset="utf-8">
        <title>Leia Basic Shadow Effect Example</title>
        <style type="text/css">
            body {
                overflow: hidden;
            }
        </style>
        <script src="https://www.leiainc.com/build/LeiaCore-latest.min.js"></script>
        <script src="https://www.leiainc.com/build/LeiaCoreUtil/js/helvetiker_regular.typeface.js"></script>
        <script src="https://www.leiainc.com/build/LeiaCoreUtil/js/helvetiker_bold.typeface.js"></script>
    </head>
    <body style="margin: 0 0 0 0;"></body>
    <script>
        // Build The LeiaCore Objects
        var leiaDisplayInfo = new LeiaDisplayInfo('https://www.leiainc.com/build/LeiaCore/config/displayPrototypeSmall.json');
        var leiaHoloScreen  = new LeiaHoloScreen(leiaDisplayInfo);
        var leiaRenderer    = new LeiaRenderer(leiaDisplayInfo, leiaHoloScreen);
        var scene = new THREE.Scene();

        // Connect LeiaRenderer To The Page
        document.body.appendChild(leiaRenderer.renderer.domElement);

        // Add New Geometries
        var boxGeometry = new THREE.BoxGeometry(3, 3, 3);
        var sphereGeometry = new THREE.SphereGeometry(2.0, 32, 32);
        var torusGeometry = new THREE.TorusGeometry(1.5, 1, 16, 50);
        var cylinderGeometry = new THREE.CylinderGeometry(1.5, 1.5, 3, 32);

        // Add More Materials
        var greenMaterial = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
        var redMaterial = new THREE.MeshBasicMaterial( { color: 0xff0000 } );
        var blueMaterial = new THREE.MeshBasicMaterial( { color: 0x2194ce } );
        var whiteMaterial = new THREE.MeshBasicMaterial( { color: 0x00ffff } );

        // Create New Meshes
        var cube = new THREE.Mesh(boxGeometry, greenMaterial);
        var sphere = new THREE.Mesh(sphereGeometry, redMaterial);
        var torus = new THREE.Mesh(torusGeometry, blueMaterial);
        var cylinder = new THREE.Mesh(cylinderGeometry, whiteMaterial);

        // Set Shape Positions
        sphere.position.z = 2.0;
        cube.position.x = 5;
        cube.position.z = 1.5;
        torus.position.x = 5;
        torus.position.y = -5;
        torus.position.z = 1;
        cylinder.position.z = 1.5;
        cylinder.position.y = -4.5;

        // Set Shape Rotations
        cylinder.rotation.x = 1.57;

        // Add New Shapes To The Scene
        scene.add(cube);
        scene.add(sphere);
        scene.add(torus);
        scene.add(cylinder);

        function animate() {
            // Tell The Browser What To Loop To Animate
            requestAnimationFrame(animate);

            // Animate The Sphere
            sphere.position.z = 2*(1.5+0.5*Math.cos(Date.now() * 0.008));

            // Render The Scene
            leiaRenderer.render(scene, leiaHoloScreen);
        }

        window.onload = function () {
            animate();
        };
    </script>
    </html>
    
## Add New Geometries ##

First in order for the objects in our scene to have something to cast a shadow against so we can actually see what we've added, lets create one new shape, a plane, and add it to our scene.

First lets add a new geometry that will represent our new plane. Add this line of code right underneath all of the other geometry statements in our example that looks like this:

    var planeGeometry = new THREE.PlaneGeometry(20, 20, 32);
    
Now your code block of geometries should look like this:

    // Add New Geometries
    var boxGeometry = new THREE.BoxGeometry(3, 3, 3);
    var sphereGeometry = new THREE.SphereGeometry(2.0, 32, 32);
    var torusGeometry = new THREE.TorusGeometry(1.5, 1, 16, 50);
    var cylinderGeometry = new THREE.CylinderGeometry(1.5, 1.5, 3, 32);
    var planeGeometry = new THREE.PlaneGeometry(20, 20, 32);

## Add More Materials ##

Next lets add one new material for our new plane. Add the following live of code right underneath the rest of your material statements that looks like this:

    var planeMaterial = new THREE.MeshBasicMaterial( {color: 0x777777, side: THREE.DoubleSide} );
    
**Note:** We also need to update the materials that our other four objects were originally generated with in order to cast and receive shadows. Lets swap our original MeshBasicMaterial()s for much more awesome MeshPhongMaterial()s. We also want to add a property to each of them, mainly "shininess: 30".

Do both of these things by locating the "Add More Materials" code block, and replacing every call to MeshBasicMaterial with MeshPhongMaterial instead, and adding ", shininess: 30" right after the "color" property definition.
    
Your materials code block should now look like this:

    // Add More Materials
    var greenMaterial = new THREE.MeshPhongMaterial({ color: 0x00ff00, shininess: 30 });
    var redMaterial = new THREE.MeshPhongMaterial( { color: 0xff0000, shininess: 30 } );
    var blueMaterial = new THREE.MeshPhongMaterial({ color: 0x2194ce, shininess: 30 });
    var whiteMaterial = new THREE.MeshPhongMaterial( { color: 0x00ffff, shininess: 30 } );
    var planeMaterial = new THREE.MeshBasicMaterial( {color: 0x777777, side: THREE.DoubleSide} );

## Create New Meshes ##

Now that we've added a new geometry and a new material for our plane, lets create the mash that will represent it in our scene. Do this by adding the following line right underneath the rest of your mesh statements that looks like:

    var plane = new THREE.Mesh(planeGeometry, planeMaterial);
    
Afterwards, your mesh code block should look just like this:

    // Create New Meshes
    var cube = new THREE.Mesh(boxGeometry, greenMaterial);
    var sphere = new THREE.Mesh(sphereGeometry, redMaterial);
    var torus = new THREE.Mesh(torusGeometry, blueMaterial);
    var cylinder = new THREE.Mesh(cylinderGeometry, whiteMaterial);
    var plane = new THREE.Mesh(planeGeometry, planeMaterial);

## Add New Shapes To The Scene ##

Now that we've create a mesh to represent our new plan, we still need to add it to our scene for it to get rendered. Add the following line right underneath the last scene.add() statement in your code block your already adding objects to the scene in that looks like this:

    scene.add(plane);
    
Your scene addition code block should now look just like this:

    // Add New Shapes To The Scene
    scene.add(cube);
    scene.add(sphere);
    scene.add(torus);
    scene.add(cylinder);
    scene.add(plane);

## Enable Shadows ##

Great! We've added a plane for our scene objects to cast a shadow against, but we still need to turn the actual shadow effect on for each of our objects. Otherwise they won't know they need to cast a shadow. This is accomplished by toggling the castShadow property on each one we want to cast a shadow.

Add the following lines directly underneath the position and rotation blocks that we don't need to alter in this example that looks just like this:

    // Enable Shadows
    cube.castShadow = true;
    sphere.castShadow = true;
    torus.castShadow = true;
    cylinder.castShadow = true;
    cylinder.receiveShadow = true;
    plane.castShadow = true;
    plane.receiveShadow = true;
    
## Add Light Sources ##

Now that we have a plane to see our new shadows against, and have enabled shadows being cast from the objects in our scene, we need to add a light source that will tell our renderer what direction and intensity to generate shadows from our scene objects.

Lets add two light sources so our scene is well illuminated, an AmbientLight, and a DirectionalLight. We'll add our AmbientLight first. Do this by adding the following statements right after our shadow enabling code block we just wrote that look just like this:

    // Add Light Sources
    var ambientLight = new THREE.AmbientLight(0x606060);
    scene.add(ambientLight);

Next lets add a bright directional light to give our shadows a very prominent and obvious direction from our scene objects. Do this by adding the following statements immediately underneath the ambient light code block we just added above like this:

    var directionalLight = new THREE.SpotLight(0xeeeeee);
    directionalLight.position.set(20, 40, 40);
    directionalLight.shadowMapWidth = directionalLight.shadowMapHeight = 512;
    directionalLight.castShadow = true;
    directionalLight.shadowDarkness = 0.7;
    directionalLight.shadowCameraVisible = true;
    scene.add(directionalLight);

If you noticed, we also added the ambient light and the directional light directly to our scene in the same code blocks we defined the lights and their properties in. This is just to keep all behaviors associated with creating and manipulating our light sources right next to each other for cleanliness and simplicity.

Thats it! That easy! 

Your entire HTML file should look like the example below now. If you load this code in your web browser, you should see the animated scene from the last example, except now with a plane underneath it, and some handsome shadows being cast from each object.

The [complete HTML file](https://github.com/LeiaInc/LeiaBasicShadowEffect/blob/master/index.html) filled in with our new 3D shapes, complete with animations and shadows, and all of our LEIA rendering code is [available here](https://github.com/LeiaInc/LeiaBasicShadowEffect/blob/master/index.html).

## Putting It All Together ##

    <!DOCTYPE html>
    <head>
        <meta charset="utf-8">
        <title>Leia Basic Shadow Effect Example</title>
        <style type="text/css">
            body {
                overflow: hidden;
            }
        </style>
        <script src="https://www.leiainc.com/build/LeiaCore-latest.min.js"></script>
        <script src="https://www.leiainc.com/build/LeiaCoreUtil/js/helvetiker_regular.typeface.js"></script>
        <script src="https://www.leiainc.com/build/LeiaCoreUtil/js/helvetiker_bold.typeface.js"></script>
    </head>
    <body style="margin: 0 0 0 0;"></body>
    <script>
        // Build The LeiaCore Objects
        var leiaDisplayInfo = new LeiaDisplayInfo('https://www.leiainc.com/build/LeiaCore/config/displayPrototypeSmall.json');
        var leiaHoloScreen  = new LeiaHoloScreen(leiaDisplayInfo);
        var leiaRenderer    = new LeiaRenderer(leiaDisplayInfo, leiaHoloScreen);
        var scene = new THREE.Scene();

        // Connect LeiaRenderer To The Page
        document.body.appendChild(leiaRenderer.renderer.domElement);

        // Add New Geometries
        var boxGeometry = new THREE.BoxGeometry(3, 3, 3);
        var sphereGeometry = new THREE.SphereGeometry(2.0, 32, 32);
        var torusGeometry = new THREE.TorusGeometry(1.5, 1, 16, 50);
        var cylinderGeometry = new THREE.CylinderGeometry(1.5, 1.5, 3, 32);
        var planeGeometry = new THREE.PlaneGeometry(20, 20, 32);

        // Add More Materials
        var greenMaterial = new THREE.MeshPhongMaterial({ color: 0x00ff00, shininess: 30 });
        var redMaterial = new THREE.MeshPhongMaterial( { color: 0xff0000, shininess: 30 } );
        var blueMaterial = new THREE.MeshPhongMaterial({ color: 0x2194ce, shininess: 30 });
        var whiteMaterial = new THREE.MeshPhongMaterial( { color: 0x00ffff, shininess: 30 } );
        var planeMaterial = new THREE.MeshBasicMaterial( {color: 0x777777, side: THREE.DoubleSide} );

        // Create New Meshes
        var cube = new THREE.Mesh(boxGeometry, greenMaterial);
        var sphere = new THREE.Mesh(sphereGeometry, redMaterial);
        var torus = new THREE.Mesh(torusGeometry, blueMaterial);
        var cylinder = new THREE.Mesh(cylinderGeometry, whiteMaterial);
        var plane = new THREE.Mesh(planeGeometry, planeMaterial);

        // Set Shape Positions
        sphere.position.z = 2.0;
        cube.position.x = 5;
        cube.position.z = 1.5;
        torus.position.x = 5;
        torus.position.y = -5;
        torus.position.z = 1;
        cylinder.position.z = 1.5;
        cylinder.position.y = -4.5;

        // Set Shape Rotations
        cylinder.rotation.x = 1.57;

        // Add New Shapes To The Scene
        scene.add(cube);
        scene.add(sphere);
        scene.add(torus);
        scene.add(cylinder);
        scene.add(plane);

        // Enable Shadows
        cube.castShadow = true;
        sphere.castShadow = true;
        torus.castShadow = true;
        cylinder.castShadow = true;
        cylinder.receiveShadow = true;
        plane.castShadow = true;
        plane.receiveShadow = true;

        // Add Light Sources
        var ambientLight = new THREE.AmbientLight(0x606060);
        scene.add(ambientLight);

        var directionalLight = new THREE.SpotLight(0xeeeeee);
        directionalLight.position.set(20, 40, 40);
        directionalLight.shadowMapWidth = directionalLight.shadowMapHeight = 512;
        directionalLight.castShadow = true;
        directionalLight.shadowDarkness = 0.7;
        directionalLight.shadowCameraVisible = true;
        scene.add(directionalLight);

        function animate() {
            // Tell The Browser What To Loop To Animate
            requestAnimationFrame(animate);

            // Animate The Sphere
            sphere.position.z = 2*(1.5+0.5*Math.cos(Date.now() * 0.008));

            // Render The Scene
            leiaRenderer.render(scene, leiaHoloScreen);
        }

        window.onload = function () {
            animate();
        };
    </script>
    </html>
