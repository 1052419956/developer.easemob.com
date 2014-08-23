---
title: 环信:开发者
layout: overview
---


<link href="/assets/css/bootstrap-2.3.2.min.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/bootstrap-responsive-2.3.2.min.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/font-awesome-3.1.0.min.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/ace.min.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/ace-responsive.min.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/ace-skins.min.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/management.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/manage.css" rel="stylesheet" type="text/css" media="screen"/>

<script src="/assets/js/jquery-1.7.2.min.js"></script>
<script src="/assets/js/bootstrap-2.3.2.min.js"></script>
<script src="/assets/js/jquery.cookie-1.3.js"></script>
<script src="/assets/js/json2.js"></script>
<script src="/assets/js/ace-elements.min.js"></script>
<script src="/assets/js/ace.min.js"></script>
<script src="/assets/js/management.js"></script>
<script src="/assets/js/layer/layer.min.js"></script>
<script type="text/javascript">
	$(function(){
		if (!getToken() || getToken()==''){
			logout();
		}else{
			getAppList();	
		}
	  });
	//function test(){
	//  if (!getToken() || getToken()==''){
	//		logout();
	//	}else{
	//		getAppList();	
	//	}
	//}
	//window.onload=test();
</script>

<div id="main-container" class="container-fluid"> <a href="javascript:void(0);" id="menu-toggler"> <span></span> </a>
  <div id="sidebar">
    <div id="sidebar-shortcuts">
      <div style="min-height: 40px;" id="sidebar-shortcuts-large"> </div>
      <div style="min-height: 40px;" id="sidebar-shortcuts-mini"> </div>
    </div>
    <ul class="nav nav-list">
			<li class="active"> <a href="/console/app_list" target="_self"> <i class="icon-ambulance"></i> <span>我的应用</span> </a></li>
			<li> <a href="/console/admin_home" target="_self"> <i class="icon-user"></i> <span>个人信息</span> </a></li>
    </ul>
    <div id="sidebar-collapse"> <i class="icon-double-angle-left"></i> </div>
  </div>
  <div class="clearfix" id="main-content">
    <div id="breadcrumbs">
      <ul class="breadcrumb">
        <li> <i class="icon-home"></i>  我的应用 </li>
      </ul>
    </div>
    <div class="clearfix" id="page-content">
      <div class="row-fluid">
      	<div class="pagination pagination-left">
      		<ul>
        		<li> <a href="/console/app_create">创建应用</a> </li>
      		</ul>
    		</div>
        <div class="row-fluid">
          <table class="table table-striped table-bordered table-hover">
            <thead>
              <tr>
								<th class="hidden-480 text-center"><i></i>应用名称</th>
                <th class="hidden-240 text-center"><i></i>用户总数</th>
                 <!--<th class="hidden-480 text-center"><i></i>新增用户[今天/昨天]</th>
               	<th class="hidden-480 text-center"><i></i>在线用户[今天/昨天]</th>
               	<th class="hidden-480 text-center"><i></i>活跃用户[今天/昨天]</th>-->
               	<th class="hidden-480 text-center"><i></i>状态</th>
               <!--<th class="hidden-480 text-center"><i></i>操作</th>-->
              </tr>
            </thead>
            <tbody id="appListBody"></tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</div>