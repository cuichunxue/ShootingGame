# 🎯 Gesture Shooting Math Game

A mobile-first, gesture-based math learning game for 2nd graders using Three.js and MediaPipe Hands.

## 🌟 Features

### Core Gameplay
- **Gesture-Based Shooting**: Use pistol hand gestures to shoot math answers
- **Strategy-Based Learning**: Rewards students for following mental math strategies
- **Multi-Player Support**: Solo, 2-Player Simultaneous, and Party modes
- **Mobile-First Design**: Optimized for iOS Safari and Android Chrome
- **Camera Privacy**: Camera feed is never displayed or saved

### Game Modes

1. **Solo Mode**: Single player, race against time
2. **2-Player Simultaneous**: Two players using both hands, compete in real-time
3. **Party Mode**: Turn-based multiplayer with leaderboard

### Math Content (2nd Grade Level)

#### Addition - Make-10 Strategy
- Example: `7 + 8 = ?`
- Strategy: `7 + 3 = 10`, then `10 + 5 = 15`
- Intermediate target: `10` (bonus points!)

#### Subtraction - Count-up Strategy
- Example: `15 - 8 = ?`
- Strategy: Think "8 + ? = 15"
- Answer: `7`

#### Multiplication - Decomposition
- Example: `7 × 6 = ?`
- Strategy: `(7 × 2) + (7 × 4) = 14 + 28 = 42`
- Intermediate targets: `14`, `28` (bonus points!)

#### Multiplication - 9× Shortcut
- Example: `9 × 7 = ?`
- Strategy: `(10 × 7) - 7 = 70 - 7 = 63`
- Intermediate targets: `70`, `7` (bonus points!)

#### Division - Inverse Multiplication
- Example: `24 ÷ 6 = ?`
- Strategy: Think "6 × ? = 24"
- Answer: `4`

## 🚀 Quick Start

1. **Open the game**: Open `gesture-math-game.html` in a mobile browser
2. **Grant camera permission**: Required for hand tracking (never shown or saved)
3. **Choose mode**: Solo, 2-Player, or Party
4. **Make a pistol gesture**:
   - Point with your index finger
   - Thumb up
   - Close thumb and index together to "arm"
   - Quickly separate to "fire"
5. **Shoot correct answers** and strategy intermediates for bonus points!

## 🎮 Gesture Controls

### Hand Gesture State Machine

The game uses a robust finite state machine (FSM) with hysteresis to prevent false triggers:

```
Idle → Aiming → Armed → Fired → Cooldown → Aiming
```

#### State Transitions

1. **Idle**: No hand detected or fingers not in position
2. **Aiming**: Index finger extended, thumb up, distance < 0.16
3. **Armed**: Thumb-index distance < 0.08 (close together)
4. **Fired**: Distance suddenly > 0.15 AND thumb velocity > 0.015
5. **Cooldown**: 250ms cooldown before next shot

#### Hysteresis

- Built-in 0.02 hysteresis prevents jitter
- Velocity threshold ensures intentional firing
- Cooldown prevents accidental double-shots

### Visual Feedback

- **Red Laser**: Player 1 (Left hand) aim indicator
- **Green Laser**: Player 2 (Right hand) aim indicator
- **Target Colors**:
  - 🟢 Green: Correct answer
  - 🟡 Gold: Strategy intermediate (smart bonus)
  - ⚪ Gray: Wrong answer (penalty)

## 📊 Scoring System

- **Correct Answer**: +100 points, streak increases
- **Strategy Intermediate**: +50 points (smart bonus), streak increases
- **Wrong Answer**: -20 points, streak decreases
- **Miss**: No score change
- **Accuracy**: Tracked as hits/shots %

## 🛠️ Technical Details

### Libraries

- **Three.js r128**: 3D rendering engine
- **MediaPipe Hands 0.4.1646424915**: Hand tracking (PINNED for stability)
- **Web Audio API**: iOS-safe sound effects

### Performance Optimizations

- **Rendering**: 60 FPS target
- **Hand Detection**: 12 FPS (configurable)
- **Pixel Ratio**: Capped at 2× for mobile performance
- **Object Pooling**:
  - 20 pre-allocated target meshes
  - 200 pre-allocated particle meshes
  - Reuse instead of create/destroy

### Mobile-First Features

- Safe-area insets for notched devices (iPhone X+)
- Touch event handling (no scroll, no zoom, no double-tap)
- Portrait-first orientation
- Fullscreen layout
- Visibility API integration (pause when hidden)

### Browser Support

- ✅ iOS Safari 14+
- ✅ Android Chrome 90+
- ✅ Desktop Chrome/Firefox (for testing)

### Privacy & Security

- Camera access required for hand tracking ONLY
- Video feed is **completely hidden** (positioned off-screen)
- No data collection, storage, or transmission
- All processing happens locally on device
- No external API calls beyond CDN libraries

## 🏗️ Architecture

### Core Systems

```
┌─────────────────────────────────────────┐
│  Three.js Scene (60 FPS)                │
│  - 3D targets with flying numbers       │
│  - Particle system (explosions, VFX)    │
│  - Laser beams (aim indicators)         │
│  - Grid environment                     │
└─────────────────────────────────────────┘
           ▲                    │
           │ render             │ world coords
           │                    ▼
┌─────────────────────────────────────────┐
│  MediaPipe Hands (12 FPS)               │
│  - Hand landmark detection              │
│  - Stable hand ID assignment            │
│  - Handedness tracking (Left/Right)     │
└─────────────────────────────────────────┘
           ▲                    │
           │ video frames       │ landmarks
           │                    ▼
┌─────────────────────────────────────────┐
│  Gesture FSM (per hand)                 │
│  - Distance calculation                 │
│  - Velocity tracking                    │
│  - State transitions + hysteresis       │
│  - Cooldown management                  │
└─────────────────────────────────────────┘
                    │
                    │ fire events
                    ▼
┌─────────────────────────────────────────┐
│  Game Logic                             │
│  - Raycasting (shot → target)           │
│  - Score calculation                    │
│  - Question generation                  │
│  - Multi-player handling                │
└─────────────────────────────────────────┘
```

### Game Flow

```
Start Screen
    ↓
Choose Mode (Solo/2-Player/Party)
    ↓
Loading (Camera + MediaPipe Model)
    ↓
Gameplay (10 questions, 60 seconds)
    ↓
Results & Leaderboard
    ↓
Next Round OR Main Menu
```

### Detection & Render Loops

**Detection Loop (12 FPS)**
```
1. Send video frame to MediaPipe
2. Receive hand landmarks (max 2 hands)
3. Assign stable hand IDs (Left/Right)
4. Update gesture FSM for each hand
5. Generate fire events
6. Update laser beams
```

**Render Loop (60 FPS)**
```
1. Update target positions (flying, bouncing)
2. Update particle physics (explosions)
3. Update timers and HUD
4. Render Three.js scene
5. Check win/lose conditions
```

## 🎓 Educational Design

### Target Audience
2nd grade students (ages 7-8) learning mental math strategies.

### Learning Objectives

1. **Mental Math Fluency**: Practice arithmetic without paper
2. **Strategy Recognition**: Identify efficient calculation methods
3. **Number Sense**: Understand number relationships
4. **Problem Solving**: Choose optimal solution paths

### Pedagogical Approach

- **Implicit Learning**: Strategies rewarded through gameplay, not explained
- **Immediate Feedback**: Visual and audio cues for all actions
- **Progressive Difficulty**: Multiple question types
- **Gamification**: Points, streaks, accuracy tracking
- **Social Learning**: Multi-player modes encourage peer learning

### Strategy Examples

**Make-10 (Addition)**
- Instead of `8 + 7 = ?`
- Think: `8 + 2 = 10`, then `10 + 5 = 15`
- This builds number decomposition skills

**Count-up (Subtraction)**
- Instead of `14 - 9 = ?`
- Think: `9 + ? = 14`
- This builds inverse operation understanding

**9× Shortcut (Multiplication)**
- Instead of memorizing `9 × 6 = 54`
- Think: `(10 × 6) - 6 = 60 - 6 = 54`
- This builds pattern recognition

## 🔧 Customization

### Adjust Gesture Sensitivity

Edit `CONFIG.GESTURE` in the code:

```javascript
GESTURE: {
    ARM_THRESHOLD: 0.08,      // Distance to arm (lower = more sensitive)
    FIRE_THRESHOLD: 0.15,     // Distance to fire (higher = harder to fire)
    HYSTERESIS: 0.02,         // Anti-jitter (lower = more responsive)
    V_MIN_THUMB: 0.015,       // Min thumb speed (lower = easier to fire)
    COOLDOWN_MS: 250          // Cooldown time (lower = faster shooting)
}
```

### Adjust Game Settings

```javascript
CONFIG: {
    MAX_TARGETS: 8,              // Number of targets per question
    QUESTIONS_PER_ROUND: 10,     // Questions before round ends
    TIME_LIMIT: 60,              // Seconds per round
    TARGET_POOL_SIZE: 20,        // Pre-allocated targets
    PARTICLE_POOL_SIZE: 200      // Pre-allocated particles
}
```

### Change Detection Rate

```javascript
detectionInterval: 1000 / 12  // 12 FPS (change to 10-15 FPS range)
```

## 🐛 Troubleshooting

### Camera Issues

**Camera not working**
- Ensure browser has camera permission
- Check if another app is using camera
- On iOS, disable Low Power Mode
- Try Safari instead of Chrome on iOS

**Hand not detected**
- Ensure good lighting (face a window/light)
- Keep hand in frame (arm's length from camera)
- Avoid busy backgrounds
- Try different hand positions

### Performance Issues

**Low FPS / Laggy**
- Close other browser tabs
- Reduce screen brightness
- Update browser to latest version
- Try on newer device
- Check CPU usage in DevTools

**Hand tracking slow**
- Normal! Detection runs at 12 FPS (intentional)
- Rendering still at 60 FPS
- Landmarks reused between detections

### Gesture Recognition

**Gun won't fire**
- Make clearer pistol shape (thumb perpendicular to index)
- Ensure thumb and index start close together
- Snap thumb away quickly (velocity matters!)
- Wait for cooldown (250ms between shots)

**Accidental firing**
- Increase `FIRE_THRESHOLD` in config
- Increase `V_MIN_THUMB` for stricter velocity check
- Increase `COOLDOWN_MS` for longer delay

### Audio Issues

**No sound**
- Tap screen to resume AudioContext (iOS requirement)
- Check device volume and mute switch
- Enable sound in browser settings
- Try with headphones

**Sound delayed**
- Normal on some iOS devices
- Web Audio API has some latency
- Cannot be fixed without native app

### Game Issues

**Targets not appearing**
- Check browser console for errors
- Refresh page
- Clear browser cache

**Wrong scoring**
- Strategy intermediates give +50 (intentional)
- Shooting gray targets gives -20 (intentional)
- Missing gives 0 points (intentional)

## 📝 Development Notes

### Version Locking (CRITICAL)

MediaPipe Hands is **PINNED** to version `0.4.1646424915`. This is critical for stability:

```javascript
locateFile: (file) => {
    return `https://unpkg.com/@mediapipe/hands@0.4.1646424915/${file}`;
}
```

**Do NOT update this version** without extensive testing, as it can cause:
- WASM initialization failures
- Model loading crashes
- Landmark detection errors

### Error Handling

All critical operations wrapped in try-catch:
- MediaPipe initialization
- Camera access
- Hand detection loop
- Audio playback

User-friendly error messages with retry options.

### Object Pooling Benefits

- **Performance**: No GC pauses from constant allocation/deallocation
- **Stability**: Pre-allocated memory prevents OOM on mobile
- **Smoothness**: Consistent frame times

### Safe-Area Insets

For notched devices (iPhone X+):

```css
padding-top: env(safe-area-inset-top);
padding-bottom: env(safe-area-inset-bottom);
padding-left: env(safe-area-inset-left);
padding-right: env(safe-area-inset-right);
```

Ensures UI doesn't go under notch or home indicator.

## 🎉 Credits

- **Three.js**: 3D rendering engine by Mr.doob and contributors
- **MediaPipe**: Hand tracking technology by Google
- **Concept**: Strategy-based math learning through gesture gaming
- **Design**: Mobile-first educational game design

## 📄 License

Educational use only. Not for commercial distribution without permission.

---

**Made with ❤️ for young mathematicians**

*Shoot numbers, learn strategies, have fun!*
