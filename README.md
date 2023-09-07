This is a Blender script for animating shape keys and exporting them as GLB. By animating, you can control the animation with PlayCanvas's AnimStateGraph.

## How to Use

### 1. Open Blender

The environment in which the operation was confirmed is Blender 3.4.1.

![](/docs/blender.png)

### 2. Import FBX

![](/docs/blender-2.png)

For example, download this [3D model](https://booth.pm/ja/items/3818504) and import it into Blender.

### 3. Switch to Script View

![](/docs/blender-scripting.png)

Select "Scripting" and switch the view.

### 4. Execute the Script

![](/docs/blender-scripting-2.png)

- Select the object.
- Set the destination directory.
- Run the script.


```python
import bpy

output_dir = "your_directory_path"  # Please set the save path
# Select the object.
obj = bpy.context.object

# Get the list of shape keys.
shape_keys = obj.data.shape_keys.key_blocks

# Specify the number of shape keys (currently 30).
loop_count = min(30, len(shape_keys) - 1)

# Create animations for each shape key and export them as GLB.
for key in shape_keys[1:1+loop_count]:  # Skip the Basis and limit the loop to 5 times.

    # Create and assign a new action.
    action_name = f"Action_{key.name}"
    action = bpy.data.actions.new(action_name)
    obj.data.shape_keys.animation_data_create()
    obj.data.shape_keys.animation_data.action = action
    
    # Reset all shape key values to 0.
    for k in shape_keys[1:]:
        k.value = 0.0
        k.keyframe_insert(data_path="value", index=-1, frame=0)
    
    # Set animation for the target shape key.
    key.value = 1.0
    key.keyframe_insert(data_path="value", index=-1, frame=24)
    
    # Export as GLB.
    export_path = f"{output_dir}/{key.name}.glb"  # Please set the save path
    bpy.ops.export_scene.gltf(filepath=export_path, export_format='GLB')
    
    # Remove the created action.
    bpy.data.actions.remove(action)
```

## Export Results

![](/docs/export-1.png)

Import this data into PlayCanvas and control the animation with AnimStateGraph.

The project set up in PlayCanvas can be found here:

- **Project URL:** [https://playcanvas.com/project/1135434](https://playcanvas.com/project/1135434)
- **Execution URL:** [https://playcanv.as/p/Q7eDemqq/](https://playcanv.as/p/Q7eDemqq/)

### Model Used

かなﾘぁさんち 【オリジナル3Dモデル】-ハオラン-HAOLAN [https://booth.pm/ja/items/3818504](https://booth.pm/ja/items/3818504)