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


<script src="/assets/js/jquery-1.7.2.min.js"></script>
<script src="/assets/js/jquery.cookie-1.3.js"></script>
<script src="/assets/js/bootstrap-2.3.2.min.js"></script>
<script src="/assets/js/json2.js"></script>
<script src="/assets/js/ace-elements.min.js"></script>
<script src="/assets/js/ace.min.js"></script>
<script src="/assets/js/management.js"></script>
<script tyep="text/javascript">
	var appUuid = getQueryString('appUuid');
	$(function(){
		if (!getToken() || getToken()==''){
			logout();
		}
		
		getAppProfile(appUuid);
		getAppChatrooms(appUuid);
	});	
	
	// 上一页数据
	function getPrevAppChatroomList(){
		getAppChatrooms(appUuid,'forward');
	}
	// 下一页数据
	function getNextAppChatroomList(){
		getAppChatrooms(appUuid,'next');
	}
	
	// 应用概述页
	function toApppofile(){
	
		window.location.href = '/console/app_profile/index.html?appUuid='+appUuid;
	}
	
	// 用户管理页
	function toAppUsersPage(){
		window.location.href = '/console/app_users/index.html?appUuid='+appUuid;
	}
	
	// 群组管理页
	function togroupAppAdmin(){
		window.location.href = '/console/app_group/index.html?appUuid='+appUuid;
	}
	
	// 推送证书管理页
	function toAppCredentialsPage(){
		window.location.href = '/console/app_credentials/index.html?appUuid='+appUuid;
	}
	// 数据统计页面
	function toApppofileCounter(){
		window.location.href = '/console/app_profile_counter/index.html?appUuid='+appUuid;
	}
	
	// 应用管理员创建页面
	function toCreateAppAdmin(){
		window.location.href = '/console/app_admin_create/index.html?appUuid='+appUuid;
	}
	
	//管理员列表页面
	function toAppUserAdmin(){
		window.location.href = '/console/app_users_admin/index.html?appUuid='+appUuid;
	}
</script>


<div>

<div id="main-container" class="container-fluid"> <a href="javascript:void(0);" id="menu-toggler"> <span></span> </a>
  <div id="sidebar">
    <div id="sidebar-shortcuts">
      <div style="min-height: 40px;" id="sidebar-shortcuts-large"> </div>
      <div style="min-height: 40px;" id="sidebar-shortcuts-mini"> </div>
    </div>
    <ul class="nav nav-list">
			<li class="active"> <a href="/console/app_list" target="_self"> <i class="icon-ambulance"></i> <span>我的应用</span> </a></li>
			<li> <a href="/console/admin_home" target="_self"> <i class="icon-user"></i> <span>个人信息</span> </a></li>
			<li> <a href="javascript:toApppofile()" target="_self"> <i class="icon-info-sign"></i><span>应用概况</span> </a></li>
			<li> <a href="javascript:toAppUsersPage()" target="_self"> <i class="icon-user-md"></i><span>IM用户</span> </a></li>
			<li> <a href="javascript:togroupAppAdmin()" target="_self"> <i class="icon-group"></i><span>群组</span> </a></li>
			<li> <a href="javascript:toAppCredentialsPage()" target="_self"> <i class="icon-fighter-jet"></i><span>推送证书</span> </a></li>
			<li> <a href="javascript:toApppofileCounter()" target="_self"> <i class="icon-bar-chart"></i><span>统计数据</span> </a></li>
			<li> <a href="javascript:toAppUserAdmin()" target="_self"> <i class="icon-cog"></i><span>App管理</span> </a></li>
    </ul>
    <div id="sidebar-collapse"> <i class="icon-double-angle-left"></i> </div>
  </div>
  <div class="clearfix" id="main-content">
    <div id="breadcrumbs">
      <ul class="breadcrumb">
         <li> <i class="icon-home"></i> 我的应用 <span class="divider"> <i class="icon-angle-right"></i> </span> </li>
         <li> <a href="javascript:void(0);" target="_self"> <span id="showName"></span></a></li>
      </ul>
    </div>
    <div class="clearfix" id="page-content">
      <div class="row-fluid">
      	
        <div class="row-fluid">
          <table class="table table-striped table-bordered table-hover">
            <thead>
              <tr>
								<th class="hidden-480 text-center"><i></i>群组名称</th>
								<th class="hidden-480 text-center"><i></i>管理员</th>
                <th class="hidden-240 text-center"><i></i>成员数</th>
                <th class="hidden-480 text-center"><i></i>描述</th>
               	<th class="hidden-480 text-center"><i></i>类型</th>
               	<th class="hidden-480 text-center"><i></i>创建时间</th>
                <th class="hidden-480 text-center"><i></i>操作</th>
              </tr>
            </thead>
            <tbody id="appChatroomBody"></tbody>
          </table>
          <div id="pagina" class="pagination pagination-right">
          	<ul>
              <li> <a href="javascript:void(0);" onClick="getPrevAppChatroomList();">上一页</a> </li>
              <li> <a href="javascript:void(0);" onClick="getNextAppChatroomList();">下一页</a> </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

</div>
