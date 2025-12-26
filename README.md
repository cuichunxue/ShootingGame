# 🎯 Gesture Shooting Math Game

A mobile-first, gesture-based math learning game for 2nd graders using Three.js and MediaPipe Hands.

## 🌟 Features

- **Gesture-Based Shooting**: Use pistol hand gestures to shoot math answers
- **Strategy-Based Learning**: Rewards students for following mental math strategies
- **Mobile-First**: Optimized for iOS Safari and Android Chrome
- **Camera Privacy**: Camera feed is never displayed or saved
- **Multi-Player Support**: Party mode with leaderboard
- **Rich VFX**: Particles, explosions, laser beams, and feedback
- **Crash-Safe**: Robust error handling with retry mechanisms

## 🚀 Quick Start

1. **Open the game**: Simply open `gesture-math-game.html` in a mobile browser
2. **Grant camera permission**: Required for hand tracking (your image is never shown)
3. **Make a pistol gesture**:
   - Point with your index finger
   - Thumb up
   - Close the gap between thumb and index to "arm" the gun
   - Quickly open the gap to "fire"
4. **Shoot the correct answers** to score points!

## 📚 How It Works

### Math Strategy Learning

The game teaches mental math strategies through gameplay:

- **Make Ten**: For addition like 7 + 8, think (7 + 3) + 5 = 15
- **Doubles**: For 6 + 7, think 6 + 6 + 1 = 13
- **Compensation**: For 29 + 9, think 29 + 10 - 1 = 38
- **Subtract by Adding**: For 15 - 8, think "what + 8 = 15?"
- **Subtract in Chunks**: For 35 - 12, think 35 - 10 - 2 = 23

### Scoring System

- **Correct Answer**: +100 points, streak increases
- **Strategy Intermediate**: +50 points (smart bonus), streak increases
- **Wrong Answer**: -20 points, streak decreases
- **Miss**: No score change

### Streak Bonuses

Build a streak by answering correctly. Higher streaks unlock visual rewards!

## 🎮 Game Controls

### Hand Gesture States

1. **Idle**: No hand detected or fingers not in position
2. **Aiming**: Index finger extended, thumb up
3. **Armed**: Thumb and index finger close together (< 8cm)
4. **Fired**: Thumb and index quickly separate (> 15cm with speed)
5. **Cooldown**: 300ms wait before next shot

### Visual Feedback

- **Green Targets**: Correct answer
- **Yellow Targets**: Strategy intermediate numbers (bonus points)
- **Gray Targets**: Wrong answers (penalty)
- **Red/Green Lasers**: Aim indicators for each hand
- **Particle Explosions**: Hit feedback

## 🛠️ Technical Details

### Libraries

- **Three.js r128**: 3D rendering engine
- **MediaPipe Hands 0.4.1646424915**: Hand tracking (pinned version for stability)

### Performance

- **Rendering**: 60 FPS target
- **Hand Detection**: 12 FPS (configurable)
- **Pixel Ratio**: Capped at 2x for performance

### Browser Support

- ✅ iOS Safari 14+
- ✅ Android Chrome 90+
- ✅ Desktop Chrome/Firefox (for testing)

### Privacy

- Camera access required for hand tracking only
- Video feed is **hidden** and never displayed
- No data is collected, stored, or transmitted
- All processing happens locally on device

## 🏗️ Architecture

### Core Systems

1. **Three.js Scene**: 3D rendering with targets, particles, and effects
2. **MediaPipe Integration**: Hand landmark detection
3. **Gesture FSM**: State machine for pistol gesture recognition
4. **Math Engine**: Question generation with strategy paths
5. **Audio System**: iOS-safe Web Audio API for sound effects
6. **UI System**: Overlays for Start, Loading, Error, HUD, Results
7. **VFX System**: Particles, lasers, explosions

### Game Flow

```
Start Screen → Camera Permission → Loading Model → Gameplay → Results → Next Round
```

### Detection Loop (12 FPS)

```
Video Frame → MediaPipe → Hand Landmarks → Gesture FSM → Shooting Events
```

### Render Loop (60 FPS)

```
Update Targets → Update Particles → Update VFX → Render Scene → Update HUD
```

## 🎓 Educational Design

### Target Audience

2nd grade students (ages 7-8) learning mental math strategies.

### Learning Objectives

1. **Mental Math Fluency**: Practice arithmetic without paper
2. **Strategy Recognition**: Identify efficient calculation methods
3. **Number Sense**: Understand relationships between numbers
4. **Problem Solving**: Choose optimal paths to solutions

### Pedagogical Approach

- **Implicit Learning**: Strategies are rewarded, not explained
- **Immediate Feedback**: Visual and audio cues for all actions
- **Progressive Difficulty**: Questions adapt to grade level
- **Gamification**: Points, streaks, and competition motivate practice

## 🔧 Customization

### Adjust Detection Sensitivity

In the code, modify `GESTURE_CONFIG`:

```javascript
const GESTURE_CONFIG = {
    ARM_THRESHOLD: 0.08,    // Distance to arm gun
    FIRE_THRESHOLD: 0.15,   // Distance to fire
    V_MIN: 0.01,            // Minimum thumb speed
    COOLDOWN_MS: 300        // Cooldown between shots
};
```

### Change Game Duration

Modify the time limit:

```javascript
STATE.timeLeft = 60; // Seconds per round
```

### Adjust Question Count

```javascript
if (STATE.questionIndex >= 10) { // Change 10 to desired count
    endRound();
}
```

## 🐛 Troubleshooting

### Camera Not Working

- Ensure browser has camera permission
- Check if another app is using the camera
- Try reloading the page
- On iOS, ensure you're not in Low Power Mode

### Hand Not Detected

- Ensure good lighting
- Keep hand in frame
- Try moving hand closer/farther from camera
- Avoid complex backgrounds

### Performance Issues

- Close other browser tabs
- Reduce screen brightness
- Update your browser
- Try on a newer device

### Audio Not Playing

- Tap the screen to resume audio context (iOS requirement)
- Check device volume
- Ensure browser allows audio
- Try headphones if device speaker is broken

## 📝 Development Notes

### Version Locking

MediaPipe Hands is pinned to `0.4.1646424915` to prevent WASM crashes. Do not update without thorough testing.

### Error Handling

All critical operations are wrapped in try-catch blocks with user-friendly error messages and retry options.

### Mobile Optimizations

- `touch-action: none` prevents scroll/bounce
- `playsinline` for video on iOS
- Pixel ratio capped at 2x
- Web Audio context resumed on user interaction

## 🎉 Credits

- **Three.js**: 3D rendering
- **MediaPipe**: Hand tracking technology by Google
- **Design**: Mobile-first gesture gaming for education

## 📄 License

Educational use only. Not for commercial distribution.

---

**Made with ❤️ for young mathematicians**
