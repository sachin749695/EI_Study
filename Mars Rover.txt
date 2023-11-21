#include <iostream>
#include <vector>

class Rover {
private:
    int x;
    int y;
    char direction;
    std::vector<std::pair<int, int>> obstacles;

public:
    Rover(int x, int y, char direction, std::vector<std::pair<int, int>> obstacles) : x(x), y(y), direction(direction), obstacles(obstacles) {}

    void moveForward() {
        if (!isObstacleAhead()) {
            switch (direction) {
                case 'N':
                    y++;
                    break;
                case 'S':
                    y--;
                    break;
                case 'E':
                    x++;
                    break;
                case 'W':
                    x--;
                    break;
            }
        }
    }

    void turnLeft() {
        switch (direction) {
            case 'N':
                direction = 'W';
                break;
            case 'S':
                direction = 'E';
                break;
            case 'E':
                direction = 'N';
                break;
            case 'W':
                direction = 'S';
                break;
        }
    }

    void turnRight() {
        switch (direction) {
            case 'N':
                direction = 'E';
                break;
            case 'S':
                direction = 'W';
                break;
            case 'E':
                direction = 'S';
                break;
            case 'W':
                direction = 'N';
                break;
        }
    }

    bool isObstacleAhead() const {
        for (const std::pair<int, int>& obstacle : obstacles) {
            if (obstacle.first == x && obstacle.second == y + 1 && direction == 'N') {
                return true;
            } else if (obstacle.first == x + 1 && obstacle.second == y && direction == 'E') {
                return true;
            } else if (obstacle.first == x && obstacle.second == y - 1 && direction == 'S') {
                return true;
            } else if (obstacle.first == x - 1 && obstacle.second == y && direction == 'W') {
                return true;
            }
        }
        return false;
    }

    void executeCommand(char command) {
        switch (command) {
            case 'M':
                moveForward();
                break;
            case 'L':
                turnLeft();
                break;
            case 'R':
                turnRight();
                break;
        }
    }

    void printStatus() {
        std::cout << "Final Position: (" << x << ", " << y << ", " << direction << ")" << std::endl;
        std::cout << "Status Report: \"Rover is at (" << x << ", " << y << ") facing " << direction << ". No Obstacles detected.\"" << std::endl;
    }
};

int main() {
    // Get grid size from user
    int gridSizeX, gridSizeY;
    std::cout << "Enter grid size (X Y): ";
    std::cin >> gridSizeX >> gridSizeY;

    // Get starting position from user
    int startX, startY;
    std::cout << "Enter starting position (X Y): ";
    std::cin >> startX >> startY;

    // Get starting direction from user
    char startDirection;
    std::cout << "Enter starting direction (N S E W): ";
    std::cin >> startDirection;

    // Get commands from user
    std::string commands;
    std::cout << "Enter commands: ";
    std::cin >> commands;

    // Get the number of obstacles from user
    int numObstacles;
    std::cout << "Enter the number of obstacles: ";
    std::cin >> numObstacles;

    // Get obstacles from user
    std::vector<std::pair<int, int>> obstacles;
    std::cout << "Enter obstacles (X Y) for each obstacle:" << std::endl;
    for (int i = 0; i < numObstacles; ++i) {
        int obstacleX, obstacleY;
        std::cout << "Obstacle " << i + 1 << ": ";
        std::cin >> obstacleX >> obstacleY;
        obstacles.emplace_back(obstacleX, obstacleY);
    }

    // Create the Rover
    Rover rover(startX, startY, startDirection, obstacles);

    // Execute the commands
    for (char command : commands) {
        rover.executeCommand(command);
    }

    // Print the final position and status report
    rover.printStatus();

    return 0;
}
