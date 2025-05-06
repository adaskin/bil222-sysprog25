all: server drone_client view

server: server.c list.c model.c json_utils.c
    gcc -o $@ $^ -lpthread -ljson-c

drone_client: drone_client.c list.c json_utils.c
    gcc -o $@ $^ -ljson-c

view: view.c model.c list.c
    gcc -o $@ $^ -lSDL2