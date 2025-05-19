import pygame
import random

# Initialize Pygame
pygame.init()

# Display setup
WIDTH, HEIGHT = 800, 600
WIN = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("AI-Based Autonomous Navigation Robot - Path Tracking")

# Colors
WHITE = (255, 255, 255)
RED = (200, 0, 0)
BLUE = (0, 100, 255)
GREEN = (0, 200, 0)
GRAY = (180, 180, 180)

# Clock
clock = pygame.time.Clock()
FPS = 60

# Robot class
class Robot:
    def _init_(self, x, y):
        self.pos = pygame.Vector2(x, y)
        self.speed = 2
        self.path = []  # To store the path

    def draw(self, win):
        pygame.draw.circle(win, BLUE, (int(self.pos.x), int(self.pos.y)), 10)
        # Draw path
        if len(self.path) > 1:
            for i in range(1, len(self.path)):
                pygame.draw.line(win, GRAY, self.path[i-1], self.path[i], 2)

    def move_towards(self, target):
        direction = (target - self.pos).normalize()
        self.pos += direction * self.speed
        self.path.append(self.pos.xy)  # Save position for path

    def distance_to(self, target):
        return self.pos.distance_to(target)

# Generate random tasks
def generate_tasks(n):
    return [pygame.Vector2(random.randint(50, WIDTH-50), random.randint(50, HEIGHT-50)) for _ in range(n)]
 Main simulation loop
def main():
    run = True
    robot = Robot(100, 100)
    tasks = generate_tasks(5)
    current_task = 0

    while run:
        WIN.fill(WHITE)
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                run = False

        if current_task < len(tasks):
            target = tasks[current_task]
            if robot.distance_to(target) > 5:
                robot.move_towards(target)
            else:
                print(f"Task {current_task + 1} completed at {target}")
                current_task += 1
 #Draw tasks
        for i, task in enumerate(tasks):
            color = RED if i == current_task else GREEN
            pygame.draw.circle(WIN, color, (int(task.x), int(task.y)), 8)

        robot.draw(WIN)
        pygame.display.update()
        clock.tick(FPS)

    pygame.quit()

main()
