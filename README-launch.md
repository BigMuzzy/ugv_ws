# Launch Commands Reference

## Remote RViz for Navigation

When running robot algorithms on the onboard computer (RPI5) and you want to launch only RViz on your laptop/desktop to visualize the navigation:

### Option 1: Using display.launch.py (Recommended)
This launches RViz with the navigation configuration along with necessary robot state publishers:

```bash
ros2 launch ugv_description display.launch.py use_rviz:=true rviz_config:=nav_2d
```

**Parameters:**
- `use_rviz:=true` - Enables RViz visualization
- `rviz_config:=nav_2d` - Uses the same navigation RViz config as `slam_nav.launch.py`

**What it launches:**
- RViz2 with navigation config
- robot_state_publisher
- joint_state_publisher

### Option 2: RViz Only (Minimal)
If you only want RViz without additional nodes:

```bash
rviz2 -d $(ros2 pkg prefix ugv_nav)/share/ugv_nav/rviz/view_nav_2d.rviz
```

**Note:** This requires that necessary topics are already being published from the RPI5.

### Available RViz Configurations

The following rviz_config options are available for `display.launch.py`:

- `description` - Basic robot model view (default)
- `bringup` - Robot bringup view
- `slam_2d` - 2D SLAM visualization
- `slam_3d` - 3D SLAM visualization
- `nav_2d` - 2D Navigation visualization
- `nav_3d` - 3D Navigation visualization

### Example Usage

For 3D navigation visualization:
```bash
ros2 launch ugv_description display.launch.py use_rviz:=true rviz_config:=nav_3d
```

For SLAM visualization:
```bash
ros2 launch ugv_description display.launch.py use_rviz:=true rviz_config:=slam_2d
```

---

## Troubleshooting

### JSON Decode Errors After Hard Reset

**Problem:** After a hard reset or improper shutdown, you may see JSON decode errors when launching:
```
[ugv_bringup-3] JSON decode error: Extra data: line 1 column 4 (char 3) with line: "{"T":1001,...
[ugv_bringup-3] [base_ctrl.feedback_data] unexpected error: device reports readiness to read but returned no data...
```

**Cause:** The serial communication buffer contains corrupted/fragmented data leftover from the previous session.

**Solution:** The `ugv_bringup` node has been updated to automatically clear serial buffers on startup (as of recent fix). The errors should:
1. Appear only briefly during the first 1-2 seconds after launch
2. Automatically resolve as the buffer is cleared
3. Not affect normal operation once clean data starts flowing

**If errors persist:**
- Stop all ROS nodes
- Wait 5 seconds for the serial device to fully reset
- Relaunch the nodes

**Code changes made (already applied):**
- Added buffer clearing during `BaseController` initialization (`ugv_bringup.py:58-61`)
- Changed error logging from `error` to `warning` level to reduce verbosity during recovery
- Added 100ms delay on startup to allow residual data to arrive before clearing buffers

These changes ensure smooth startup even after hard resets or improper shutdowns.
