<body style="text-align: justify">
    <h1>Como rodar o projeto:</h1>
    
    <h2>Passo 1: Configurando o ambiente</h2>
    <ol>
        <li>Tenha (ou instale) o Docker em sua máquina.</li>
        <li>
            Certifique-se de que as portas necessárias estejam disponíveis. São elas:
            <ul>
                <li>9104: mysql-exporter</li>
                <li>6379: redis</li>
                <li>8080: wordpress</li>
                <li>9090: prometheus</li>
                <li>3000: grafana</li>
                <li>8081: cadvisor</li>
            </ul>
        </li>
    </ol>

    <hr/>
    
    <h2>Passo 2: Clonar e acessar o repositório do projeto</h2>
    <ol>
        <li>
            Em uma pasta de sua preferência, abra o Git Bash e digite:
            <pre><code>git clone https://github.com/LucasDelaValentina/ProjetoFinalDocker</code></pre>
        </li>
        <li>
            Acesse a pasta do projeto:
            <pre><code>cd ProjetoFinalDocker</code></pre>
        </li>
    </ol>

    <hr/>
    
    <h2>Passo 3: Executar o projeto</h2>
    <ol>
        <li>
            Inicie o Docker Swarm:
            <pre><code>docker swarm init</code></pre>
        </li>
        <li>
            Execute o comando para iniciar os serviços:
            <pre><code>docker stack deploy -c docker-compose.yml "nome_stack"</code></pre>
            Serviços incluídos:
            <ul>
                <li>mysql - database</li>
                <li>mysql-exporter - exporta dados do mysql para o prometheus</li>
                <li>redis - cache para queries do wordpress</li>
                <li>wordpress - site</li>
                <li>prometheus - monitoramento</li>
                <li>grafana - dashboards</li>
                <li>cadvisor - monitoramento do docker</li>
            </ul>
        </li>
    </ol>

    <hr/>
    
    <h2>Passo 4: Configurar o Wordpress</h2>
    <ol>
        <li>Acesse: <code>http://localhost:8080</code></li>
        <li>Configure o Wordpress seguindo as instruções.</li>
        <li>
            Após a configuração, acesse a página de administração:
            <img src="./md/image_wp_admin.png"/>
        </li>
        <li>
            Vá em "Plugins" para ver os plugins instalados:
            <img src="./md/image_wp_plugins.png"/>
        </li>
        <li>
            Adicione o plugin "Redis Object Cache":
            <img src="./md/image_wp_adicionar_plugin.png"/>
        </li>
        <li>
            Instale e ative o plugin Redis Object Cache:
            <img src="./md/image_wp_redis.png"/>
        </li>
    </ol>

    <hr/>
    
    <h2>Corrigir erro de conexão entre o Redis e o Docker</h2>
    <ol>
        <li>
            No terminal, digite <code>sudo su</code> e faça login como root. Em seguida, digite <code>docker ps</code> para listar os containers em execução. Copie o CONTAINER ID do Wordpress.
        </li>
        <li>
            Execute o comando para acessar o container:
            <pre><code>docker exec -it "ID DO SEU CONTAINER" bash</code></pre>
        </li>
        <li>
            Atualize e instale o nano:
            <pre><code>apt update && apt install nano</code></pre>
        </li>
        <li>
            Edite o arquivo wp-config.php:
            <pre><code>nano wp-config.php</code></pre>
        </li>
        <li>
            Adicione as linhas antes de <code>/* That's all, stop editing! Happy publishing. */</code>:
            <pre><code>define('WP_CACHE', true);<br>define('WP_REDIS_HOST', 'redis');<br>define('WP_REDIS_PORT', 6379);</code></pre>
        </li>
        <li>
            Salve (Ctrl+O) e feche (Ctrl+X) o arquivo. Atualize a página de plugins e ative o cache do Redis:
            <img src="./md/image_wp_redis_acessivel.png"/>
        </li>
    </ol>

    <hr/>
    
    <h2>Passo 5: Acessar o Prometheus</h2>
    <ol>
        <li>
            Acesse: <code>http://localhost:9090</code>
        </li>
        <li>
            Consulte métricas dos containers e do MySQL, como:
            <pre>container_cpu_system_seconds_total<br>container_fs_reads_total<br>container_fs_limit_bytes<br>mysql_exporter_collector_success<br>mysql_global_status_connections<br>mysql_global_status_max_used_connections</pre>
        </li>
    </ol>

    <hr/>
    
    <h2>Passo 6: Dashboard do Grafana</h2>
    <ol>
        <li>
            Acesse: <code>http://localhost:3000</code> (usuário: admin, senha: admin)
            <img src="./md/image_grafana_login.png"/>
        </li>
        <li>
            Configure o data source do Prometheus:
            <img src="./md/image_grafana_datasource.png"/>
            <img src="./md/image_grafana_prometheus.png"/>
        </li>
        <li>
            Crie uma dashboard e adicione visualizações com métricas do Prometheus:
            <img src="./md/image_grafana_setqueries.png"/>
        </li>
    </ol>

    <hr/>
    
    <h2>Passo Bônus: Cadvisor</h2>
    <ol>
        <li>
            Acesse os gráficos dos containers e do Docker em: <code>http://localhost:8081</code>
        </li>
    </ol>
</body>
