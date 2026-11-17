Evidencias:
Pontos:

1 Historico:
*   89e00cc (HEAD -> main, github/main) Merge pull request #1 from ricardo21fonte/feature/api-docker
|\
| * bf10c12 (github/feature/api-docker, feature/api-docker) app. py com o health
| * 00bd670 teste endpoint
| * 7fe4509 resolvido erro ci.yml
| * 9b9d3b2 adicionar ci.yml workflow
| * c0a0f55 adicionar app.py testes/test.app e docker files
|/
*   19f1bdf (origin/main, origin/HEAD) Integra documentação de operações
|\
| * 5f8bf77 (origin/docs/operacoes) verificacao de logs e estado do container
|/
*   7756ad4 Resolve conflito no estado do serviço
|\
| * 68fab12 (fix/estado-servico) fix: definir estado saudavel
* | 01af357 chore: sinalizar estado de aviso
|/
* c81fcd2 (feature/healthcheck) Adiciona ficheiro de health check
* 69f360c (healthcheck) preparacao dos ficheiros

2: Branches e remotos: 
 git branch -a
  feature/api-docker
  feature/healthcheck
  fix/estado-servico
  healthcheck
* main
  remotes/github/feature/api-docker
  remotes/github/main
  remotes/origin/HEAD -> origin/main
  remotes/origin/docs/operacoes
  remotes/origin/main

git remote -v
    github  https://github.com/ricardo21fonte/app-suporte-lab.git (fetch)
    github  https://github.com/ricardo21fonte/app-suporte-lab.git (push)
    origin  ../app-suporte-remoto.git (fetch)
    origin  ../app-suporte-remoto.git (push)

3: Tag: 
    commit 19f1bdf580f4fd161c3a30928cf01250f0f147a8 (tag: v1.0.0, origin/main, origin/HEAD)

4: Docker:
$ curl -i http://127.0.0.1:8000/health
HTTP/1.0 200 OK
Server: BaseHTTP/0.6 Python/3.13.18
Date: Tue, 17 Nov 2026 09:36:02 GMT
Content-Type: text/plain; charset=utf-8
Content-Length: 15

status=HEALTHY

5: CI
Commit analisado: 89e00cc
Job validar: sucesso
Job construir: sucesso
Resultado do smoke test: Run docker run -d --name app-ci -p 127.0.0.1:18000:8000 app-suporte:ci 
6d0bada7a084949049cba434bcee326dc9506ab68843d853850017185966f899
curl: (56) Recv failure: Connection reset by peer
status=HEALTHY
Smoke test OK

6: TABELA:

Commit que introduziu a regressão : 00bd670

Testes unitários  :test_estado_invalido (test_app.HealthTests.                test_estado_invalido) ... ok
test_estado_saudavel (test_app.HealthTests.test_estado_saudavel) ... ok

Construçao da imagem Docker : PASSA

Resposta HTTP do endpoint /health  : 404 NOT FOUND

Job ou etapa com falha : construir (smoke test)

Mensagem principal dos logs: 404

Causa identificada : healthcheck e o smoke test continuam a pedir /health


7: Questoes:

1-git add prepara para serem registadas, o git commit grava as alterações preparadas no histórico do Git com uma descriçao.

2-O Git não sabe qual corresponde o ficehiro correto, por isso é necessário decidir que conteúdo manter ou juntar.

3-app-suporte tem uma pasta de trabalho para editar ficheiros.app-suporte-remoto.git guarda o histórico e as referências do Git, mas não tem pasta de trabalho (como repositório remoto)

4-git fetch descarrega atualizações do remoto sem as integrar na branch atual. git pull descarrega e tenta integrá-las. fetch é preferível quando se quer rever os commits ou as diferenças antes de decidir como integrar as alterações no projeto

5-Não. Uma tag identifica um commit ou uma versão do projeto. Adiciona ao projeto exige um processo de deployment separado.

6-Não. Os testes verificam apenas se is_healthy() aceita ou rejeita determinados textos. Não iniciam o servidor nem fazem pedidos HTTP. Um teste do serviço em execução verifica se o endpoint responde através da rede.

7-Porque o build confirma que a imagem pôde ser construída, mas não confirma que o container arranca ou que a aplicação responde ou que o endpoint funciona corretamente.

8-O smoke test pode detetar problemas de integração ou execução, como o container não arrancar ou  a porta não estar acessível ou o endpoint /health não devolver uma resposta HTTP de sucesso.

9- Os testes unitários verificam is_healthy(), mas não fazem um pedido HTTP. Por isso podem passar mesmo que a rota esteja errada. O smoke test inicia o container e consulta /health, testando o serviço integrado.
10-Comunica o hash do commit, a branch e a versão/imagem testada; os passos para reproduzir o problema; o endpoint e método usados; o resultado esperado e o observado, incluindo código HTTP e corpo; os resultados dos testes e da pipeline; e os logs relevantes. Distingue claramente factos observados de hipóteses.