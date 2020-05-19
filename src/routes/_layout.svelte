
<script>
  import H6 from '@smui/common/H6.svelte';
  import Drawer, {AppContent, Content, Header, Subtitle, Scrim} from '@smui/drawer';
  import Button, {Label} from '@smui/button';
  import List, {Item, Text, Graphic, Separator, Subheader} from '@smui/list';
  import {onMount} from 'svelte';
  import {stores} from '@sapper/app';
  import {mdiFileDocument, mdiCodeTags, mdiTwitter, mdiGithub} from '@mdi/js';
  import './_app.scss';
  import TopAppBar, {Row, Section, Title} from '@smui/top-app-bar';
  
  import IconButton from '@smui/icon-button';
  import {Icon} from '@smui/common';
  import A from '@smui/common/A.svelte';
  const {page} = stores();
  const iframe = $page.path.startsWith('/demo/top-app-bar-iframe');
  let mainContent;
  let miniWindow = false;
  let drawerOpen = false;

/*
const sections = [
    {
      name: 'Buttons',
      route: '/demo/button',
      indent: 0,
      repos: ['https://github.com/hperrin/svelte-material-ui/tree/master/packages/button']
    },
    {
      name: 'Floating Action Button',
      route: '/demo/fab',
      indent: 1,
      repos: ['https://github.com/hperrin/svelte-material-ui/tree/master/packages/fab']
    },
    {
      name: 'Icon Buttons',
      route: '/demo/icon-button',
      indent: 1,
      repos: ['https://github.com/hperrin/svelte-material-ui/tree/master/packages/icon-button']
    },
    {
      name: 'Cards',
      route: '/demo/card',
      indent: 0,
      repos: ['https://github.com/hperrin/svelte-material-ui/tree/master/packages/card']
    },
    {
      name: 'Chips',
      route: '/demo/chips',
      indent: 0,
      repos: ['https://github.com/hperrin/svelte-material-ui/tree/master/packages/chips']
    },
    {
      name: 'Data Tables',
      route: '/demo/data-table',
      indent: 0,
      repos: ['https://github.com/hperrin/svelte-material-ui/tree/master/packages/data-table']
    },
    {
      name: 'Typography',
      route: '/demo/typography',
      indent: 0
    }
  ];


  //let activeSection = sections.find(section => 'route' in section && section.route === $page.path);
  //$: repos = (activeSection && 'repos' in activeSection) ? activeSection.repos : [];

  function pickSection(section) {
    drawerOpen = false;
    mainContent.scrollTop = 0;
    // Svelte/Sapper is not updated the components correctly, so I need this.
    sections.forEach(section => section.component.$set({activated: false}));
    section.component.$set({activated: true});
    activeSection = 'shortcut' in section ? sections.find(sec => sec.route === section.shortcut) : section;
  }
*/
  
  onMount(setMiniWindow);
  
  function setMiniWindow() {
    miniWindow = window.innerWidth < 720;
  }
</script>

<svelte:window on:resize={setMiniWindow} />
{#if iframe}
  <slot></slot>
{:else}
  <TopAppBar variant="static" class="demo-top-app-bar">
    <Row>
      <Section>
        <IconButton class="material-icons" on:click={() => drawerOpen = !drawerOpen}>menu</IconButton>
        <Title component={A} href="/" class="mdc-theme--primary" style="{miniWindow ? 'padding-left: 0' : ''};">
          <img src="logo.png" alt="logo" height="52" />
        </Title>
      </Section>
      <Section align="end" toolbar>
        <IconButton href="https://twitter.com/SciActive">
          <Icon>
            <svg style="width:24px;height:24px" viewBox="0 0 24 24">
              <path fill="#fff" d="{mdiTwitter}" />
            </svg>
          </Icon>
        </IconButton>
        <IconButton href="https://github.com/hperrin/svelte-material-ui">
          <Icon>
            <svg style="width:24px;height:24px" viewBox="0 0 24 24">
              <path fill="#fff" d="{mdiGithub}" />
            </svg>
          </Icon>
        </IconButton>
      </Section>
    </Row>
  </TopAppBar>
  <div class="drawer-container">
    <Drawer variant={miniWindow ? 'modal' : null} bind:open={drawerOpen} class="demo-drawer mdc-theme--secondary-bg {miniWindow ? 'demo-drawer-adjust' : ''}">
      <Content>
        <List>
          <Item href="javascript:void(0)">
            <Graphic class="material-icons" aria-hidden="true">home</Graphic>
            <Text>Home</Text>
          </Item>
          <Item href="javascript:void(0)">
            <Graphic class="material-icons" aria-hidden="true">star</Graphic>
            <Text>Trending</Text>
          </Item>
          <Item href="javascript:void(0)">
            <Graphic class="material-icons" aria-hidden="true">subscriptions</Graphic>
            <Text>Subscriptions</Text>
          </Item>
          <Separator nav />
            <Subheader component={H6}>Subscriptions</Subheader>
            <Item href="javascript:void(0)">
              <Graphic class="material-icons" aria-hidden="true">bookmark</Graphic>
              <Text>Tengu Media</Text>
            </Item>
            <Item href="javascript:void(0)">
              <Graphic class="material-icons" aria-hidden="true">bookmark</Graphic>
              <Text>Huckabee</Text>
            </Item>
            <Item href="javascript:void(0)">
              <Graphic class="material-icons" aria-hidden="true">bookmark</Graphic>
              <Text>London Real</Text>
            </Item>
          <!--
            
          {#each sections as section (section.name)}
            <Item
              bind:this={section.component}
              href={'route' in section ? section.route : section.shortcut}
              on:click={() => pickSection(section)}
              activated={'route' in section && section.route === $page.path}
              title={section.name}
              style="{section.indent ? 'margin-left: '+(section.indent * 25)+'px;' : ''}"
            >
              <Text class="mdc-theme--on-secondary">{section.name}</Text>
            </Item>
          {/each}


          -->
          
        </List>
      </Content>
    </Drawer>

    {#if miniWindow}
      <Scrim />
    {/if}
    <AppContent class="demo-app-content">
      <main class="demo-main-content" bind:this={mainContent}>
        <slot></slot>
      </main>
    </AppContent>
  </div>
{/if}
