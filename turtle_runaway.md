개요

# 개요

이 게임은 **runner**(도망자)가 있고 **chaser**(추격자)가 도망자를 쫓습니다. 밀리초마다 잡혔는지 여부를 판단하며, 점수를 보여줍니다. 점수는 도망자가 잡히지 않았을 때 1점씩 얻을 수 있습니다. **runner**는 지능적으로 상대방과 반대 방향으로 도망가려고 하기 때문에, **chaser**는 이를 고려하여 전략을 세워야 합니다. **chaser**는 키보드의 **방향키**를 사용해 수동으로 이동할 수 있습니다.

## 주요 코드 설명

### 1. 타이머 설정 및 게임 시간 계산
```python
self.ai_timer_msec = ai_timer_msec
self.start_time = time.time()
self.canvas.ontimer(self.step, self.ai_timer_msec)
```
- `time.time()`을 호출하여 프로그램이 시작된 이후 또는 1970년 1월 1일 기준으로 경과한 시간을 반환합니다.
- 이를 통해 게임 시간이 경과한 시간을 추적할 수 있습니다. 예를 들어, `game_time = time.time() - self.start_time`을 계산하여 현재 경과한 게임 시간을 알 수 있습니다.

### 2. 점수 계산
```python
if not is_catched:
    self.score += 1
self.drawer.setpos(-300, 240)
self.drawer.write(f"Score: {self.score}")
```
- 이 부분은 점수를 계산하는 코드입니다. 도망자가 잡히지 않았을 때마다 점수가 1점씩 증가합니다.
- 이 코드는 100밀리초마다 반복되며, 도망자가 잡히지 않으면 점수가 계속 올라갑니다.

### 3. 지능적인 움직임을 위한 `Intelligent_Mover` 클래스
```python
class Intelligent_Mover(turtle.RawTurtle):
    def __init__(self, canvas, step_move=10, step_turn=10):
        super().__init__(canvas)
        self.step_move = step_move
        self.step_turn = step_turn

    def run_ai(self, opp_pos, opp_heading):
        mypos = self.pos()
        if mypos[0] < opp_pos[0]:
            self.left(self.step_turn)
        elif mypos[0] > opp_pos[0]:
            self.right(self.step_turn)
        self.forward(self.step_turn)
```
- 이 코드는 **runner**가 **지능적인 거북이**처럼 움직이도록 합니다. 
- `pos()` 함수는 거북이의 현재 위치를 반환하며, 이때 `x` 좌표 값을 이용해 상대방의 위치를 확인한 후 **반대 방향**으로 고개를 돌립니다. 
- 이를 통해 `runner`는 **chaser**가 쫓아오는 방향과 반대쪽으로 도망가려 하므로, **runner**를 잡기가 더 어려워집니다.
