# Django Celery Async Tasks

O Celery é uma **fila de tarefas distribuídas** para sistema UNIX. Ele permite que você descarregue o trabalho de seu aplicativo Python. Depois de integrar o Celery ao seu aplicativo, você pode enviar tarefas demoradas para a fila de tarefas do Celery. Dessa forma, seu aplicativo web pode continuar respondendo rapidamente aos usuários enquanto o Celery conclui operações caras de forma assíncrona em segundo plano.

O Celery é uma fila de tarefas distribuídas que pode coletar, gravar, agendar e executar tarefas fora de seu programa principal.

Para receber tarefas de seu programa e enviar resultados para um back-end, o Celery requer um agente de mensagens para comunicação. **Redis** e **RabbitMQ** são dois corretores de mensagens que os desenvolvedores costumam usar junto com o Celery.

**Observação:** Conectar o Celery a um back-end de resultados é opcional. Depois de instruir o Celery a executar uma tarefa, ele cumprirá seu dever, independentemente de você acompanhar o resultado da tarefa ou não.

No entanto, manter um registro de todos os resultados da tarefa geralmente é útil, especialmente se você estiver distribuindo tarefas para várias filas. Para manter as informações sobre os resultados da tarefa, você precisa de um back-end de banco de dados.

Você pode usar muitos bancos de dados diferentes para acompanhar os resultados das tarefas do Celery. Ao usar o Redis, você limita as dependências que precisa instalar porque ele pode assumir ambas as funções.

## Por que usar o Celery?
Existem duas razões principais pelas quais a maioria dos desenvolvedores deseja começar a usar o Celery:

- **Offloading work** (descarregar o trabalho) do seu aplicativo para processos distribuídos que podem ser executados independentemente do seu aplicativo.
- **Scheduling task execution** (agendar a execução de tarefas) em um horário específico, às vezes como eventos recorrentes.

O Celery é uma excelente escolha para esses dois casos de uso. Ele se define como "uma fila de tarefas com foco no precessamento em tempo real, além de oferecer suporte ao agendamento de tarefas".

Embora essas duas funcionalidades façam parte do Celery, elas geralmente são abordadas separadamente:

- **Celery workers** são processos de trabalho que executam tarefas independentemente umas das outras e fora do contexto de seu serviço principal.
- **Celery beat** é um agendador que orquestra quando executar tarefas. Você também pode usá-lo para agendar tarefas periódicas.

Os workers do Celery são a espinha dorsal do Celery. Mesmo que você pretenda agendar tarefas recorrentes usando o Celery beat, um *worker* do Celery receberá suas instruções e as tratará no horário agendado. O que o Celery Beat adiciona à mistura é um agendador baseado em tempo para os *workers* do Celery.

## Como você pode aproveitar o Celery para seu aplicativo Django?

O Celery não é útil apenas para aplicativos da Web, mas certamente é popular nesse contexto. Isso porque você pode enfrentar com eficiência algumas situações cotidianas no desenvolvimento da Web usando uma fila de tarefas distribuídas, como Celery:

- **Envio de e-mail:** você pode querer enviar uma verificação de e-mail, um e-mail de redefinição de senha ou uma configuração de envio de formulário. O envio de e-mails pode demorar um pouco e deixar seu app lento, principalmente se ele tiver muitos usuários.
- **Processamento de imagem:** você pode querer redimensionar as imagens de avatar que os usuários carregam ou aplicar alguma codificação em todas as imagens que os usuários podem compartilhar em sua plataforma. O processamento de imagens geralmente é uma tarefa que consome muitos recursos e pode tornar seu aplicativo mais lento, principalmente se você estiver atendendo a uma grande comunidade de usuários.
- **Processamento de texto:** se você permitir que os usuários adicionem dados ao seu aplicativo, convém monitorar a entrada deles. Por exemplo, você pode querer verificar palavrões nos comentários ou traduzir o texto enviado pelo usuário para um idioma diferente, Lidar com todo esse trabalho no contexto de seu aplicativo da Web pode prejudicar siginificamente o desempenho.
- **Chamadas de API e outras solicitações da Web:** se você precisar fazer solicitações da Web para fornecer o serviço que seu aplicativo oferece, poderá se deparar rapidamente com tempos de espera inesperados. Isso é válido tanto para solitações de API com taxa limitada quanto para outras tarefas, como web scraping. Muitas vezes, é melhor transferir essas solicitações para um processo diferente.
- **Análise de dados:** A análise de dados é notoriamente intensiva em recursos. Se seu aplicativo da web analisar dados para seus usuários, você verá rapidamente seu aplicativo não responde se estiver lidando com todo o trabalho diretamente no Django.
- **Execuções do modelo de aprendizado de máquina:** assim como em outras análises de dados, aguardar os resultados das operações de aprendizado de máquina pode demorar um pouco. Em vez de permitir que seus usuários esperem que os cálculos sejam concluídos, você pode descarregar esse trabalho no Celery para que eles possam continuar navegando em seu aplicativo da web até que os resultados voltem.
- **Gerações de relatórios:** se você estiver atendendo a um aplicativo que permite aos usuários gerar relatórios a partir dos dados fornecidos, perceberá que a criação de arquivos PDF não ocorre instantaneamente. Será uma experiência de usuário melhor se você deixar o Celery lidar com isso em segundo plano, em vez de congelar seu aplicativo da Web até que o relatório esteja pronto para download.

A configuração principal para todos esses diferentes casos de uso será semelhante. Assim que você entender como transferir processos de computação ou demorados para uma fiça de tarefas distribuídas, você liberará o Django para lidar com o ciclo de solicitação-resposta HTTP.

---
