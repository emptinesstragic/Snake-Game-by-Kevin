import pygame
import time
import random

#impordime erinevaid teeke, ilma nendeta kood ei tööta.

pygame.init()

white = (255, 255, 255)
yellow = (255, 255, 102)
black = (0, 0, 0)
red = (213, 50, 80)
green = (0, 255, 0)
blue = (50, 153, 213)

#objekti värvid

dis_width = 800
dis_height = 600

#ekraani eraldusvõime

dis = pygame.display.set_mode((dis_width, dis_height))
pygame.display.set_caption('Snake game by Kevin')

#Aknas kuvatakse faili nimi

clock = pygame.time.Clock()
snake_block = 10
snake_speed = 15

#klotside arv ja millise kiirusega madu liigub

font_style = pygame.font.SysFont("bahnschrift", 25)
score_font = pygame.font.SysFont("comicsansms", 35)

#font

def our_snake(snake_block, snake_List):
    for x in snake_List:
        pygame.draw.rect(dis, black, [x[0], x[1], snake_block, snake_block])

        #joonistab ja sisaldab madu koordinaat

def message(msg, color):
    mesg = font_style.render(msg, True, color)
    dis.blit(mesg, [dis_width / 6, dis_height / 3])

    #kirjutab kaotusteade

def gameLoop():
    game_over = False
    game_close = False

    #alustab mäng

    x1 = dis_width / 2
    y1 = dis_height / 2

    #esialgne asukoht

    x1_change = 0
    y1_change = 0

    #esialgne liikumissuund

    snake_List = []
    Length_of_snake = 1
    
    #salvestab kogu mao koordinaadid ja käivitab mao 1 lahtrist
    
    foodx = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0
    foody = round(random.randrange(0, dis_height - snake_block) / 10.0) * 10.0

    #puuviljade asukoht 10 ploki raadiuses

    while not game_over:

        while game_close:
            dis.fill(blue)
            message("Sa kaotasid! Vajutage uuesti Q - Quit või C - Play", red)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        return gameLoop()
                        
        #kui mängija kaotab, prindib ta edasised juhised

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT and x1_change == 0:  
                    x1_change = -snake_block
                    y1_change = 0
                elif event.key == pygame.K_RIGHT and x1_change == 0:  
                    x1_change = snake_block
                    y1_change = 0
                elif event.key == pygame.K_UP and y1_change == 0:  
                    y1_change = -snake_block
                    x1_change = 0
                elif event.key == pygame.K_DOWN and y1_change == 0:  
                    y1_change = snake_block
                    x1_change = 0

        if x1 >= dis_width or x1 < 0 or y1 >= dis_height or y1 < 0:
            game_close = True

            #kui mängija läheb aknast välja, siis mängija kaotab

        x1 += x1_change
        y1 += y1_change
        
        #värskendage pea koordinaate
        
        dis.fill(blue)
        pygame.draw.rect(dis, green, [foodx, foody, snake_block, snake_block])
        
        #täitke taust ühe värviga ja toit teise värviga

        snake_Head = [x1, y1]
        snake_List.append(snake_Head)
        if len(snake_List) > Length_of_snake:
            del snake_List[0]

        for x in snake_List[:-1]:  
            if x == snake_Head:
                game_close = True

                #kui madu põrkub iseendaga - kaotus

        our_snake(snake_block, snake_List)
        pygame.display.update()

        if x1 == foodx and y1 == foody:
            foodx = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0
            foody = round(random.randrange(0, dis_height - snake_block) / 10.0) * 10.0
            Length_of_snake += 1
            
            #kui madu sõi toitu - loob uut toitu ja lisab maole veel ühe raku
            
        clock.tick(snake_speed)

    pygame.quit()

    #mängu sulgemisel välju pygame'ist

gameLoop()
