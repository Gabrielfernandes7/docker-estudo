# Tecnologias do Docker

Os containers Docker utilizam tecnologias do Kernel do Linux para o isolamento e gerenciamento de recursos: namespaces para isolamento de processos, cgroups para limitação de recursos e sistemas de arquivos em união (union filesystems) para o armazenamento de camada eficiente. Essas tecnologias permitem que containers leves, portáveis e seguros que compartilham o kernel do host.

## Namespaces

Namespaces são responsáveis pelo isolamento lógico. Eles fazem com que cada container tenha sua própria visão de processos, rede, sistema de arquivo montado, hostname e usuários. Com o namespace PID, um container enxerga apenas seus próprios processos; com o NET possui sua própri pilha de rede virtual. Isso cria a sensação de ambiente independente, mesmo compartilhando o mesmo kernel.

## Cgroups 

Cgroups complementam os namespaces ao controlar consumo de recursos. Enquanto namespaces isolam, cgroups limitam. É por meio deles que se define quanto de CPU ou memória um container pode usar. Isso impede que um container comprometa todo o servidor e garante previsibilidade em ambientes de múltiplos serviços.

## Union Filesystem

Viabilizam o modelo de imagem em camadas. Cada instrução do Dockerfile cria uma nova camada. Essas camadas são reutilizadas entre imagens, reduzindo o espaço em disco e acelerando builds. Tecnologias como OverlayFD implementam esse conceito no Linux, permitindo que várias cmadas seja combinadas como se fossem um único sistema de arquivos.
