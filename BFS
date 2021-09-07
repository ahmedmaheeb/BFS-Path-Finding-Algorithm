import pygame
import math
import queue 


WIDTH = 800
WIN = pygame.display.set_mode((WIDTH,WIDTH))
pygame.display.set_caption("Bellman Ford's Path Finding Algorithm")
RED = (255, 0, 0)
YELLOW = (255,255,0)
WHITE = (255,255,255)
GREEN  = (0, 255, 0)
BLUE = (0, 0, 255)
GREY = (220, 220, 220)
BLACK = (0, 0, 0)
PURPLE=(128,0,128)
TURQUOISE = (64,224,208)
ORANGE =(255,165,0)


class Node:
    def __init__(self, row, col, width, total_rows):
        self.row = row
        self.col = col
        self.x = row*width
        self.y = col*width
        self.color = WHITE
        self.neighbours = []
        self.width = width
        self.total_rows = total_rows

    def get_pos(self):
        return self.row,self.col

    def reset(self):
        self.color= WHITE
    
    def make_start(self):
        self.color = ORANGE

    def is_closed(self):
        return self.color ==RED

    def is_open(self):
        return self.color==GREEN
    
    def make_open(self):
        self.color = GREEN

    def make_closed(self):
        self.color = RED

    def is_barrier(self):
        return self.color == BLACK

    def make_end(self):
        self.color = TURQUOISE

    def make_barrier(self):
        self.color = BLACK
    
    def make_path(self):
        self.color = PURPLE

    def draw(self, win):
        pygame.draw.rect(win, self.color,(self.x,self.y,self.width,self.width))

    def update_neighbours(self, grid):
        self.neighbours = []
        if self.row < self.total_rows -1 and not grid[self.row + 1][self.col].is_barrier():
            self.neighbours.append(grid[self.row + 1][self.col])

        if self.row >0 and not grid[self.row - 1][self.col].is_barrier():
            self.neighbours.append(grid[self.row - 1][self.col])

        if self.col < self.total_rows -1 and not grid[self.row][self.col+1].is_barrier():
            self.neighbours.append(grid[self.row][self.col+1])

        if self.col >0 and not grid[self.row ][self.col-1].is_barrier():
            self.neighbours.append(grid[self.row ][self.col-1])    

    def __lt__(self,other):
        return False

def reconstruct_path(came_from, current, draw):
    while current in came_from:
        current = came_from[current]
        current.make_path()
        draw()

def algorithm(draw, grid, start, end):
    # optimize later 
    came_from = {}
    q = queue.Queue()
    q.put(start)
    while not q.empty():
        u = q.get()

        if u == end :
            reconstruct_path(came_from,end, draw)
            return True
        for v in u.neighbours:
            if not v.is_closed():
                came_from[v]=u
                v.make_open()
                q.put(v)
        draw()
        if u !=start:
            u.make_closed()
    return False


def make_grid(rows,width):
    grid= []
    gap = width//rows
    for i in range(rows):
        grid.append([])
        for j in range(rows):
            node = Node(i,j,gap,rows)
            grid[i].append(node)
    return grid       

def draw_grid(win, rows,width):
    gap = width//rows
    for i in range(rows):
        pygame.draw.line(win, GREY,(0,i*gap),(width, i*gap))
        for j in range(rows):
            pygame.draw.line(win, GREY, (j*gap,0), (j*gap, width))

def draw(win, grid, rows, width):
    win.fill(WHITE)
    for row in grid:
        for node in row:
            node.draw(win)
  
    draw_grid(win, rows, width)
    pygame.display.update()

def get_clicked_pos(pos, rows, width):
    gap = width//rows
    y, x = pos    
    row = y // gap
    col = x// gap

    return row, col

def main(win, width):
    ROWS = 20 
    grid = make_grid(ROWS,width)

    start = None 
    end = None
    run = True
    started = False
    while run:
        draw(win, grid, ROWS, width)
        for event in  pygame.event.get():
            if event.type == pygame.QUIT:
                run = False

            if started :
                continue

            if pygame.mouse.get_pressed()[0]:
                pos = pygame.mouse.get_pos()
                row, col = get_clicked_pos(pos,ROWS, width)
                node = grid[row][col]
                if not start and node != end:
                    start = node
                    start.make_start()
                
                elif not end and node != start:
                    end = node
                    end.make_end()
                
                elif node != end and node != start:
                    node.make_barrier()

            #elif pygame.mouse.get_pressed()[2]:
             #   pos = pygame.mouse.get_pos()
              #  row,col = get_clicked_pos(pos, ROWS, width)
               # node = grid[row][col]
                #node.reset()
                #if node == start:
                #    start == None
                #elif node == end:
                 #   end = None
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE and not started :
                    for row in grid :
                        for node in row :
                            node.update_neighbours(grid)
                    
                    algorithm(lambda: draw(win, grid, ROWS, width), grid , start, end)

    pygame.quit()

main(WIN,WIDTH)