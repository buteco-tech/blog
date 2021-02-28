---
title: Estatísticas
slug: estatisticas
toc: true
disable_share: true
disable_comments: true
---

<div class="stat-widget">
  <header>
    <h2 class="title">Visualizações</h2>
    <span class="subtitle">Últimos 30 dias</span>
  </header>
  <div id="pageviews-30"><div class="no-data">No Data</div></div>
</div>

<div class="stat-widget">
  <header>
    <h2 class="title">Visualizações (Total)</h2>
    <span class="subtitle">Por mês</span>
  </header>
  <div id="pageviews-total"><div class="no-data">No Data</div></div>
</div>

<div class="stat-widget">
  <header>
    <h2 class="title">Top 10 Páginas</h2>
    <span class="subtitle">Últimos 30 dias</span>
  </header>
  <div id="top-10-pageviews-30"><div class="no-data">No Data</div></div>
</div>

<div class="stat-widget">
  <header>
    <h2 class="title">Top 10 Páginas</h2>
    <span class="subtitle">Últimos 90 dias</span>
  </header>
  <div id="top-10-pageviews-90"><div class="no-data">No Data</div></div>
</div>

<div class="stat-widget">
  <header>
    <h2 class="title">Usuários vs Sessão</h2>
    <span class="subtitle">Últimos 30 dias</span>
  </header>
  <div id="sessions-users-30"><div class="no-data">No Data</div></div>
</div>

<div class="stat-widget">
  <header>
    <h2 class="title">Usuários vs Sessão (Total)</h2>
    <span class="subtitle">Por mês</span>
  </header>
  <div id="sessions-users-total"><div class="no-data">No Data</div></div>
</div>

<div class="stat-widget">
  <header>
    <h2 class="title">Tempo médio</h2>
    <span class="subtitle">Últimos 90 dias</span>
  </header>
  <div id="avg-time-90"><div class="no-data">No Data</div></div>
</div>

<div class="stat-widget">
  <header>
    <h2 class="title">Aquisição</h2>
    <span class="subtitle">Últimos 30 dias</span>
  </header>
  <div id="acquisition"><div class="no-data">No Data</div></div>
</div>

<div class="stat-widget">
  <header>
    <h2 class="title">Top 10 Países</h2>
    <span class="subtitle">Últimos 30 dias</span>
  </header>
  <div id="top-10-contries"><div class="no-data">No Data</div></div>
</div>

<div class="stat-widget">
  <header>
    <h2 class="title">Top Browsers</h2>
    <span class="subtitle">Últimos 30 dias</span>
  </header>
  <div id="top-browsers"><div class="no-data">No Data</div></div>
</div>

<!-- <script>
(function(w,d,s,g,js,fs){
  g=w.gapi||(w.gapi={});g.analytics={q:[],ready:function(f){this.q.push(f);}};
  js=d.createElement(s);fs=d.getElementsByTagName(s)[0];
  js.src='https://apis.google.com/js/platform.js';
  fs.parentNode.insertBefore(js,fs);js.onload=function(){g.load('analytics');};
}(window,document,'script'));
</script> -->

<script type="text/javascript">
  function onGoogleLoad() {

    gapi.load('analytics', _ => {
    window.google2 = window.google;
      analyticsReady();
    });
  }

  function analyticsReady() {

  var userSessionTotal = new gapi.analytics.googleCharts.DataChart({
    query: {
      'ids': 'ga:92397045',
      'start-date': '120daysAgo',
      'end-date': 'yesterday',
      'metrics': 'ga:users,ga:newUsers,ga:sessions',
      'dimensions': 'ga:yearMonth'
    },
    chart: {
      'container': 'sessions-users-total',
      'type': 'COLUMN',
      'options': {
        'width': '100%',
      }
    }
  });

  var pageViewsTotal = new gapi.analytics.googleCharts.DataChart({
    query: {
      'ids': 'ga:92397045',
      'start-date': '120daysAgo',
      'end-date': 'yesterday',
      'metrics': 'ga:pageviews',
      'dimensions': 'ga:yearMonth'
    },
    chart: {
      'container': 'pageviews-total',
      'type': 'LINE',
      'options': {
        'width': '100%',
      }
    }
  });

  var pageViews30 = new gapi.analytics.googleCharts.DataChart({
    query: {
      'ids': 'ga:92397045',
      'start-date': '30daysAgo',
      'end-date': 'yesterday',
      'metrics': 'ga:pageviews',
      'dimensions': 'ga:date'
    },
    chart: {
      'container': 'pageviews-30',
      'type': 'LINE',
      'options': {
        'width': '100%',
      }
    }
  });

  var avgTime90 = new gapi.analytics.googleCharts.DataChart({
    query: {
      'ids': 'ga:92397045',
      'start-date': '90daysAgo',
      'end-date': 'yesterday',
      'metrics': 'ga:avgTimeOnPage,ga:avgSessionDuration',
      'dimensions': 'ga:yearMonth'
    },
    chart: {
      'container': 'avg-time-90',
      'type': 'LINE',
      'options': {
        'width': '100%',
      }
    }
  });

  var userSession30 = new gapi.analytics.googleCharts.DataChart({
    query: {
      'ids': 'ga:92397045',
      'start-date': '30daysAgo',
      'end-date': 'yesterday',
      'metrics': 'ga:users,ga:newUsers,ga:sessions',
      'dimensions': 'ga:date'
    },
    chart: {
      'container': 'sessions-users-30',
      'type': 'LINE',
      'options': {
        'width': '100%'
      }
    }
  });

  var top10pageviews90 = new gapi.analytics.googleCharts.DataChart({
    query: {
      'ids': 'ga:92397045',
      'start-date': '90daysAgo',
      'end-date': 'yesterday',
      'metrics': 'ga:pageviews',
      'dimensions': 'ga:pagePathLevel1',
      'sort': '-ga:pageviews',
      'filters': 'ga:pagePathLevel1!=/',
      'max-results': 10
    },
    chart: {
      'container': 'top-10-pageviews-90',
      'type': 'PIE',
      'options': {
        'width': '100%',
        'pieHole': 4/9,
      }
    }
  });

  var top10pageviews30 = new gapi.analytics.googleCharts.DataChart({
    query: {
      'ids': 'ga:92397045',
      'start-date': '30daysAgo',
      'end-date': 'yesterday',
      'metrics': 'ga:pageviews',
      'dimensions': 'ga:pagePathLevel1',
      'sort': '-ga:pageviews',
      'filters': 'ga:pagePathLevel1!=/',
      'max-results': 10
    },
    chart: {
      'container': 'top-10-pageviews-30',
      'type': 'PIE',
      'options': {
        'width': '100%',
        'pieHole': 4/9,
      }
    }
  });

  var acquisition = new gapi.analytics.googleCharts.DataChart({
    query: {
      'ids': 'ga:92397045',
      'start-date': '30daysAgo',
      'end-date': 'yesterday',
      'metrics': 'ga:pageviews',
      'dimensions': 'ga:channelGrouping',
      'sort': '-ga:pageviews',
      'max-results': 10
    },
    chart: {
      'container': 'acquisition',
      'type': 'PIE',
      'options': {
        'width': '100%',
        'pieHole': 4/9,
      }
    }
  });

  var top10contries = new gapi.analytics.googleCharts.DataChart({
    query: {
      'ids': 'ga:92397045',
      'start-date': '30daysAgo',
      'end-date': 'yesterday',
      'metrics': 'ga:sessions',
      'dimensions': 'ga:country',
      'sort': '-ga:sessions',
      'max-results': 10
    },
    chart: {
      'container': 'top-10-contries',
      'type': 'PIE',
      'options': {
        'width': '100%',
        'pieHole': 4/9,
      }
    }
  });

  var topBrowsers = new gapi.analytics.googleCharts.DataChart({
    query: {
      'ids': 'ga:92397045',
      'start-date': '30daysAgo',
      'end-date': 'yesterday',
      'dimensions': 'ga:browser',
      'metrics': 'ga:sessions',
      'sort': '-ga:sessions',
      'max-results': 6
    },
    chart: {
      'type': 'TABLE',
      'container': 'top-browsers',
      'options': {
        'width': '100%'
      }
    }
  });

  gapi.analytics.auth.on('signIn', function() {
    userSessionTotal.execute();
    pageViewsTotal.execute();
    pageViews30.execute();
    avgTime90.execute();
    userSession30.execute();
    top10pageviews90.execute();
    top10pageviews30.execute();
    acquisition.execute();
    top10contries.execute();
    topBrowsers.execute();
  });

  fetch("https://estatisticas.buteco.tech/token").then((resp) => resp.json()).then(function(data) {
    gapi.analytics.auth.authorize({
      'serverAuth': {
        'access_token': data['token']
      }
    });
  });
  }
</script>

<script src="https://apis.google.com/js/client.js?onload=onGoogleLoad"></script>
<script>
if (!window.google || !window.google.load) {
  var tag = document.createElement('script');
  tag.type = 'text/javascript';
  tag.src = 'https://www.google.com/jsapi';
  var s = document.getElementsByTagName('script')[0];
  s.parentNode.insertBefore(tag, s);
}
</script>
