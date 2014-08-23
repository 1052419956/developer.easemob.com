---
title: 环信:开发者
layout: overview
---

<link href="/assets/css/bootstrap-2.3.2.min.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/bootstrap-responsive-2.3.2.min.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/font-awesome-3.1.0.min.css" rel="stylesheet" type="text/css" media="screen"/>


<link href="/assets/css/ace.min.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/ace-responsive.min.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/ace-skins.min.css" rel="stylesheet" type="text/css" media="screen"/>
<!--[if lte IE 8]>
		  <link rel="stylesheet" href="/assets/css/ace-ie.min.css" />
		<![endif]-->

<link href="/assets/css/management.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/manage.css" rel="stylesheet" type="text/css" media="screen"/>


<style type="text/css">
	.row-fluid td{ text-align:center;}
	.row-fluid a{ margin:auto;}
	.row-fluid dt{float:left;}
	.row-fluid dd{float:left;}
	.a_button:link{ text-decoration:none;}
	.a_button:hover{ background-color:#2283c5;}
</style>

<script src="/assets/js/jquery-1.7.2.min.js"></script>
<script src="/assets/js/bootstrap-2.3.2.min.js"></script>
<script src="/assets/js/jquery.cookie-1.3.js"></script>
<script src="/assets/js/json2.js"></script>
<script src="/assets/js/ace-elements.min.js"></script>
<script src="/assets/js/ace.min.js"></script>
<script src="/assets/js/management.js"></script>
<script type="text/javascript">
	var appUuid;
	$(function(){
		if (!getToken() || getToken()==''){
			logout();
		}
		
		//getAppList();	
		appUuid = getQueryString('appUuid').split(',')[0];
		//var groupid=getQueryString('appUuid').split(',')[1];
		var groupid=getQueryString('groupid');
		
		getAppChatroomsuser(appUuid,groupid);
	});
		//删除选定的用户
	function deleteAppUsersBox(){
		deleteAppqunzuCheckBox(appUuid);	
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
	// 去除字符串中所有空格
	function removeAllSpace(str) {
	  	return str.replace(/\s+/g, "");
	}
	//发送消息
	function showSendMessge(){
		sendMessge(appUuid);	
	}
	
	//添加好友
  function showAddFriendHTML(){
		var appUuid = getQueryString('appUuid').split(',')[0];
		//var owner_username = getQueryString('appUuid').split(',')[1];
		var owner_username = getQueryString('groupid');
		
		showAddFriend(appUuid,owner_username);
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
			<li> <a href="/console/app_list" target="_self"> <i class="icon-ambulance"></i> <span>我的应用</span> </a></li>
			<li> <a href="/console/admin_home" target="_self"> <i class="icon-user"></i> <span>个人信息</span> </a></li>
			<li> <a href="javascript:toApppofile()" target="_self"> <i class="icon-info-sign"></i><span>应用概况</span> </a></li>
			<li> <a href="javascript:toAppUsersPage()" target="_self"> <i class="icon-user-md"></i><span>IM用户</span> </a></li>
			<li class="active"> <a href="javascript:togroupAppAdmin()" target="_self"> <i class="icon-group"></i><span>群组</span> </a></li>
			<li> <a href="javascript:toAppCredentialsPage()" target="_self"> <i class="icon-fighter-jet"></i><span>推送证书</span> </a></li>
			<li> <a href="javascript:toApppofileCounter()" target="_self"> <i class="icon-bar-chart"></i><span>统计数据</span> </a></li>
			<li> <a href="javascript:toAppUserAdmin()" target="_self"> <i class="icon-cog"></i><span>App管理</span> </a></li>
    </ul>
    <div id="sidebar-collapse"> <i class="icon-double-angle-left"></i> </div>
  </div>
  <div class="clearfix" id="main-content">
    <div id="breadcrumbs">
      <ul class="breadcrumb">
        <li> <i class="icon-home"></i>  我的应用 &nbsp; </li><li><span class="divider"> <i class="icon-angle-right"></i> </span>&nbsp;<a href="javascript:toApppofile()"><span id="showName"></span></a>&nbsp;</li><li><span class="divider"> <i class="icon-angle-right"></i> </span>&nbsp;IM用户</span>&nbsp;</li><li><span class="divider"> <i class="icon-angle-right"></i> </span>&nbsp;<span id="showUsername"></span>&nbsp;</li><li><span class="divider"> <i class="icon-angle-right"></i> </span>&nbsp;我的群组</span>&nbsp;</li>
      </ul>
    </div>
    <div class="clearfix" id="page-content">
      <div class="row-fluid">
      	<div class="pagination pagination-left">
      		
           <!-- <div class="inbox-header">
							<form id="searchForm" action="#" class="form-search pull-right">
								<div class="input-append">
									<input class="m-wrap" value="" id="userInbox" type="text" placeholder="输入完整用户名" />
									<button class="btn green" id="searchBtn" onClick="searchUserTmp()" type="button">搜索</button>
									<input value="" id="userInboxBak" type="hidden" />
                                    
								</div>
							</form>
						</div>-->
           <!--<ul>
        		<li> <a href="/console/app_create">发送消息</a></li> 
			     </ul>-->
					 &nbsp;&nbsp;&nbsp;
					 <ul>
           	<li><a href="javascript:showAddFriendHTML()" data-toggle="modal" role="button">添加成员</a> </li>
				   	<li style="display:none"> <a id="showAddFriend" href="#addFriend" data-toggle="modal" role="button"></a> </li>
					 </ul>
    		</div>
        <div class="row-fluid">
          <table class="table table-striped table-bordered table-hover">
            <thead>
              <tr>
								<th class="hidden-480 text-center"><i></i>成员名称</th>
                <th class="hidden-240 text-center"><i></i>操&nbsp;&nbsp;作</th>
                 <!--<th class="hidden-480 text-center"><i></i>新增用户[今天/昨天]</th>
               	<th class="hidden-480 text-center"><i></i>在线用户[今天/昨天]</th>
               	<th class="hidden-480 text-center"><i></i>活跃用户[今天/昨天]</th>-->
               	
               <!--<th class="hidden-480 text-center"><i></i>操作</th>-->
              </tr>
            </thead>
            <tbody id="appIMBody" style="text-align:center;position:"> 
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</div>
<!--添加成员-->
			<div id="addFriend" class="modal hide fade" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
			<div class="modal-header">
				<button id="closeButn" type="button" class="close" data-dismiss="modal" aria-hidden="true">×</button>
				<h3 id="myModalLabel">添加好友</h3>
			</div>
			<div class="modal-body">
				<div class="row-fluid">
					<div class="span12">
						<div class="control-group">
							<label class="control-label" for="messegeContent">好友名称：</label>
							<div class="controls">
                <input type="text" id="friendUsername" name="friendUsername" value=""/>
								<input type="hidden" id="usernameFriend" name="usernameFriend" value=""/>
								<input type="hidden" id="appUuidFriend" name="appUuidFriend" value=""/>
							</div>
						</div>
					</div>
				</div>
			</div>
			<div class="modal-footer"> 
				<a class="btn" onClick="addIMFriendusers();" href="javascript:void(0);">添加</a>
				<button id="messageCloseBtn" class="btn" data-dismiss="modal" onClick="clsSpan()" aria-hidden="true">关闭</button>
			</div>
		</div>
<!--添加成员end-->


</div>

