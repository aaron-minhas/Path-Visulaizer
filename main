#include <iostream>
#include <queue>
#include <SDL2/SDL.h>

using namespace std;

#define ROWS 10
#define COLS 10
#define CELL_SIZE 40

int dRow[] = {-1, 1, 0, 0};
int dCol[] = {0, 0, -1, 1};

struct Node {
    int row;
    int col;
    int dist;
};

void printGrid(char grid[ROWS][COLS]) {
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            cout << grid[i][j] << " ";
        }
        cout << endl;
    }
    cout << endl;
}

bool isValid(int row, int col, char grid[ROWS][COLS], bool visited[ROWS][COLS]) {
    return (row >= 0 && row < ROWS && col >= 0 && col < COLS && 
            grid[row][col] != '#' && !visited[row][col]);
}

void drawGrid(SDL_Renderer* renderer, char grid[ROWS][COLS], int startRow, int startCol, int endRow, int endCol) {
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            SDL_Rect cell = {j * CELL_SIZE, i * CELL_SIZE, CELL_SIZE, CELL_SIZE};

            if (i == startRow && j == startCol) {
                SDL_SetRenderDrawColor(renderer, 0, 255, 0, 255);  // Green for start
            } else if (i == endRow && j == endCol) {
                SDL_SetRenderDrawColor(renderer, 255, 0, 0, 255);  // Red for end
            } else if (grid[i][j] == '#') {
                SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);  // Black for walls
            } else if (grid[i][j] == '.') {
                SDL_SetRenderDrawColor(renderer, 0, 0, 255, 255);  // Blue for path
            } else {
                SDL_SetRenderDrawColor(renderer, 255, 255, 255, 255);  // White for empty cells
            }

            SDL_RenderFillRect(renderer, &cell);
            SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
            SDL_RenderDrawRect(renderer, &cell);
        }
    }
}

void bfs(SDL_Renderer* renderer, char grid[ROWS][COLS], int startRow, int startCol, int endRow, int endCol) {
    bool visited[ROWS][COLS] = {false};
    queue<Node> q;

    Node startNode;
    startNode.row = startRow;
    startNode.col = startCol;
    startNode.dist = 0;

    q.push(startNode);
    visited[startRow][startCol] = true;

    while (!q.empty()) {
        Node curr = q.front();
        q.pop();

        if (curr.row == endRow && curr.col == endCol) {
            cout << "Shortest path found with distance: " << curr.dist << endl;
            return;
        }

        for (int i = 0; i < 4; ++i) {
            int newRow = curr.row + dRow[i];
            int newCol = curr.col + dCol[i];

            if (isValid(newRow, newCol, grid, visited)) {
                visited[newRow][newCol] = true;

                Node newNode;
                newNode.row = newRow;
                newNode.col = newCol;
                newNode.dist = curr.dist + 1;

                q.push(newNode);
                grid[newRow][newCol] = '.';  // Mark as visited

                // Render the grid after each step
                SDL_SetRenderDrawColor(renderer, 255, 255, 255, 255);
                SDL_RenderClear(renderer);
                drawGrid(renderer, grid, startRow, startCol, endRow, endCol);
                SDL_RenderPresent(renderer);
                SDL_Delay(500);  // Add delay for visualization
            }
        }
    }

    cout << "No path found!" << endl;
}

int main(int argc, char* argv[]) {
    if (SDL_Init(SDL_INIT_VIDEO) < 0) {
        cerr << "SDL could not initialize! SDL_Error: " << SDL_GetError() << endl;
        return 1;
    }

    SDL_Window* window = SDL_CreateWindow("Pathfinding Visualizer",
                                          SDL_WINDOWPOS_CENTERED,
                                          SDL_WINDOWPOS_CENTERED,
                                          COLS * CELL_SIZE,
                                          ROWS * CELL_SIZE,
                                          SDL_WINDOW_SHOWN);
    if (!window) {
        cerr << "Window could not be created! SDL_Error: " << SDL_GetError() << endl;
        SDL_Quit();
        return 1;
    }

    SDL_Renderer* renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);
    if (!renderer) {
        cerr << "Renderer could not be created! SDL_Error: " << SDL_GetError() << endl;
        SDL_DestroyWindow(window);
        SDL_Quit();
        return 1;
    }

    char grid[ROWS][COLS] = {
        {'0', '0', '0', '0', '#', '0', '0', '0', '0', '0'},
        {'0', '#', '#', '0', '#', '0', '#', '#', '#', '0'},
        {'0', '0', '0', '0', '0', '0', '0', '#', '0', '0'},
        {'0', '#', '0', '#', '#', '#', '0', '#', '0', '0'},
        {'0', '#', '0', '0', '0', '0', '0', '0', '0', '0'},
        {'0', '0', '0', '#', '0', '#', '#', '#', '#', '0'},
        {'0', '#', '0', '#', '0', '0', '0', '0', '0', '0'},
        {'0', '0', '0', '0', '0', '#', '0', '#', '0', '0'},
        {'0', '#', '#', '#', '0', '0', '0', '#', '0', '0'},
        {'0', '0', '0', '0', '0', '0', '0', '0', '0', '0'}
    };

    int startRow = 0, startCol = 0, endRow = 0, endCol = 9;

    cout << "Initial Grid:\n";
    printGrid(grid);

//    cout << "Enter starting coordinates (row col): ";
//    cin >> startRow >> startCol;
//    cout << "Enter ending coordinates (row col): ";
//    cin >> endRow >> endCol;

    bfs(renderer, grid, startRow, startCol, endRow, endCol);

    cout << "\nFinal Grid:\n";
    printGrid(grid);
    
    // Event loop to keep the window open
    bool running = true;
    SDL_Event event;
    while (running) {
        while (SDL_PollEvent(&event)) {
            if (event.type == SDL_QUIT) {
                running = false;
            }
        }
    }

    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();

    return 0;
}
