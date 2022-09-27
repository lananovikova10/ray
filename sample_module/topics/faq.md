[//]: # (title: FAQ)


<chapter collapsible="true" title="There's a SSL error when opening the local website">

This is expected. We're enforcing HTTPS to make sure that WebXR and other modern web APIs work out-of-the-box, but that means some browsers complain that the SSL connection (between your local development server and the local website) can't be verified.

You can generate a local self-signed SSL certificate to fix this if you want.

</chapter>

<chapter collapsible="true" title="My local website stays black">

If that happens there's usually an exception either in engine code or your code. Open the dev tools (<control>Ctrl + Shift + I</control> in Chrome) and check the Console for errors.  
In some cases, especially when you just updated the Needle Engine package version, this can be fixed by stopping and restarting the local dev server.  
For that, click the running progress bar in the bottom right corner of the Editor, and click the little <control>X</control> to cancel the running task. Then, press Play again.

</chapter>

<chapter collapsible="true" title="My website becomes to large (too many MB)">

This can have many reasons, but a few common ones are:
- too many textures or textures are too large
- meshes have too many vertices
- meshes have vertex attributes you don't actually need (e.g. have normals and tangents, but you're not using them)
- objects are disabled and not ignored – disabled objects get exported as well in case you want to turn them on at runtime! Set their Tag to `EditorOnly` to completely ignore them for export.

If loading time itself is an issue you can **try to split up your content into multiple glb files** and load them on-demand (this is what we do on our website). For it to work you can put your content into Prefabs or Scenes and reference them from any of your scripts. Please have a look at [Scripting/Addressables in the documentation](./scripting.md#assetreference-and-addressables).

</chapter>

<chapter collapsible="true" title="My scripts don't work after export">

- Your existing C# code will *not* export as-is, you have to write matching typescript / javascript for it.
- Needle uses typescript / javascript for components and generates C# stubs for them.
- Components that already have matching JS will show that in the Inspector.

</chapter>

<chapter collapsible="true" title="My lightmaps look different / too bright">
Ensure you're following [best practices for lightmaps](https://docs.needle.tools/lightmaps) and read about [mixing baked and non-baked objects](https://github.com/needle-tools/needle-engine-support/blob/main/documentation/export.md#mixing-baked-and-non-baked-objects)
</chapter>

<chapter collapsible="true" title="My scene is too bright / lighting looks different than in Unity">

Make sure that your lights are set to "Baked" or "Realtime". "Mixed" is currently not supported.  

- Lights set to mixed (with lightmapping) do affect objects twice in three.js, since there is currently no way to exclude lightmapped objects from lighting
- The ``Intensity Multiplier`` factor for Skybox in ``Lighting/Environment`` is currently not supported and has no effect in Needle Engine  
  ![image](https://user-images.githubusercontent.com/5083203/185429006-2a5cd6a1-8ea2-4a8e-87f8-33e3afd080ec.png)
- Light shadow intensity can currently not be changed due to a three.js limitation.

Also see the docs on [mixing baked and non-baked objects](https://github.com/needle-tools/needle-engine-support/blob/main/documentation/export.md#mixing-baked-and-non-baked-objects).

</chapter>

<chapter collapsible="true" title="My Shadows are not visible or cut off">

Please read the following points:

- Your light has shadows enabled (either Soft Shadow or Hard Shadow)
- Your objects are set to "Cast Shadows: On" (see MeshRenderer component)
- For directional lights the position of the light is currently important since the shadow camera will be placed where the light is located in the scene.

</chapter>

<chapter collapsible="true" title="My colors look wrong">

Ensure your project is set to Linear colorspace.

</chapter>

<chapter collapsible="true" title="I'm using networking and Glitch and it doesn't work if more than 30 people visit the Glitch page at the same time">

- Deploying on Glitch is a fast way to prototype and might even work for some small productions. The little server there doesn't have the power and bandwidth to host many people in a persistent session.
- We're working on other networking ideas, but in the meantime you can host the website somewhere else (with node.js support) or simply remix it to distribute load among multiple servers. You can also host the [networking backend package](https://www.npmjs.com/package/@needle-tools/needle-tiny-networking-ws) itself somewhere else where it can scale e.g. Google Cloud.

</chapter>

<chapter collapsible="true" title="My website doesn't have AR/VR buttons">

- Make sure to add the `WebXR` component somewhere inside your root `GltfObject`.
- Optionally add a `AR Session Root` component on your root `GltfObject` or within the child hierarchy to specify placement, scale and orientation for WebXR.
- Optionally add a `XR Rig` component to control where users start in VR

</chapter>

<chapter collapsible="true" title="My local server does not start / I do not see a website">

The most likely reason is an incorrect installation.
Check the console and the `ExportInfo` component for errors or warnings.

If these warnings/errors didn't help, try the following steps in order. Give them some time to complete. Stop once your problem has been resolved. Check the console for warnings and errors.

- Make sure you follow the [Prerequisites](./getting_started.md#prerequisites-).
- Install your project by selecting your `ExportInfo` component and clicking `Install`
- Run a clean installation by selecting your `ExportInfo` component, holding Alt and clicking `Clean Install`
- Try opening your web project directory in a command line tool and follow these steps:
    - run ``npm install`` and then ``npm run dev-host``
    - Make sure both the local runtime package (``node_modules/@needle-tools/engine``) as well as three.js (``node_modules/three``) did install.
    - You may run ``npm install`` in both of these directories as well.

</chapter>

<chapter collapsible="true" title="Does C# component generation work with javascript only too?">

While generating C# components does technically run with vanilla javascript too we don't recommend it and fully support it since it is more guesswork or simply impossible for the generator to know which C# type to create for your javascript class. Below you find a minimal example on how to generate a Unity Component from javascript if you really want to tho.    

``` javascript
class MyScript extends Behaviour
{
    //@type float
    myField = 5;
}
```

</chapter>

<chapter collapsible="true" title="My objects are white after export">

This usually happens when you're using custom shaders or materials and their properties don't cleanly translate to known property names for glTF export.  
You can either make sure you're using glTF-compatible materials and shaders, or mark shaders as "custom" to export them directly.

</chapter>


## Still have questions? 😱
[Ask in our friendly discord community](https://discord.needle.tools)