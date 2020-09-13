---
title: Capturando imagem da webcam com o PyGame
date: 2014-09-09 00:27:57
authors:
  - alexandrevicenzi
slug: capturando-imagem-da-webcam-com-o-pygame
categories:
  - desenvolvimento
  - jogos
tags:
  - camera
  - pygame
  - python
  - webcam
images:
  - /images/posts/pygame-logo.png
---

{{<figure src="/images/posts/pygame-logo.png" alt="pygame">}}

O [PyGame][pygame] é uma coleção de módulos Python para a criação de jogos baseados em [SDL][sdl].
Sendo em Python, ele é portável e funciona em praticamente qualquer plataforma ou sistema.

Para instalar o PyGame execute o comando:

```sh
pip install pygame
```

Provavelmente será necessário instalar uma variedade de pacotes no seu sistema, confira a [documentação de compilação][compilation] para obter a lista completa, ou tente instalar o pacote `python3-pygame`, caso esteja disponível em sua distro, que ele virá com todas as dependências necessárias.

No exemplo de hoje vou mostrar como utilizar o PyGame e uma webcam para capturar imagens em tempo real. O módulo de câmera é experimental e suporta apenas Linux e câmeras v4l2 no momento.

Abaixo você pode verificar o código completo com alguns comentários:

```py
import pygame
import pygame.camera

from datetime import datetime
from pygame.locals import *

FPS = 30

class App(object):

    def __init__(self, size=None):
        # Iniciando o Pygame.
        pygame.init()
        # Iniciando o módulo de som.
        pygame.mixer.init()
        # Iniciando o módulo de câmera.
        pygame.camera.init()
        # Iniciando o módulo de fonte.
        pygame.font.init()
        pygame.display.set_caption("PyCam")

        self.size = size or (640, 480)
        # Cria a tela principal.
        self.display = pygame.display.set_mode(self.size, 0)
        # Relógio para controlar o FPS.
        self.clock = pygame.time.Clock()

    def __del__(self):
        # Liberando os módulos.
        pygame.font.quit()
        pygame.camera.quit()
        pygame.mixer.quit()
        pygame.quit()

    def main(self):
        running = True

        # Recuperar a lista de câmeras disponíveis.
        clist = pygame.camera.list_cameras()

        # Lista vazia?
        if not clist:
            raise ValueError("Nenhuma camera encontrada.")

        # Define a câmera a ser usada e a resolução.
        cam = pygame.camera.Camera(clist[0], self.size)
        # Iniciando a Webcam.
        cam.start()

        # Enquanto o usuário não fechar a tela ou apertar ESC,
        # ficará exibindo imagens da câmera em tempo real.
        while running:
            events = pygame.event.get()

            for e in events:
                if e.type == QUIT or (e.type == KEYDOWN and e.key == K_ESCAPE):
                    running = False
                if (e.type == KEYDOWN and e.key == K_RETURN):
                    # Salva a imagem em um arquivo em disco ao apertar a tecla ENTER.
                    img = cam.get_image()
                    filename = datetime.strftime(datetime.now(), "capture_%d_%m_%Y_%H_%m_%S.png")
                    pygame.image.save(img, filename)
                    print(f"Imagem salva em: {filename}")
                    try:
                        # Toca um som de captura, simulando uma câmera.
                        pygame.mixer.Sound("camera-shutter-click.ogg").play()
                    except Exception as e:
                        print(e)

            # Verifica se a câmera está pronta para uso.
            if cam.query_image():
                # Captura uma imagem e joga na tela.
                snapshot = cam.get_image()
                self.display.blit(snapshot, (0, 0))

            pygame.display.flip()
            self.clock.tick(FPS)

        # Liberando a Webcam.
        cam.stop()

if __name__ == "__main__":
    A = App()
    A.main()
```

Atalhos disponíveis:

<kbd>Enter</kbd> — uma imagem será capturada e salva no local de execução do aplicativo
<kbd>Esc</kbd> — encerra a aplicação

E ai, gostou? Deixe um comentário.

Até a próxima.

[pygame]: https://www.pygame.org/
[sdl]: https://www.libsdl.org/
[compilation]: https://www.pygame.org/wiki/Compilation
