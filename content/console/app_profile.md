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
<link href="/assets/css/SpryTabbedPanels.css" rel="stylesheet" type="text/css" media="screen"/>


<script src="/assets/js/jquery-1.7.2.min.js"></script>
<script src="/assets/js/jquery.cookie-1.3.js"></script>
<script src="/assets/js/bootstrap-2.3.2.min.js"></script>
<script src="/assets/js/json2.js"></script>
<script src="/assets/js/ace-elements.min.js"></script>
<script src="/assets/js/ace.min.js"></script>
<script src="/assets/js/management.js"></script>
<script tyep="text/javascript">
	var appUuid = getQueryString('appUuid');
	var Application = getQueryString('Application');
	//var appUuid=$.cookie('appUuid');
	//var Application=$.cookie('Application');
	if(Application!=null){
       $.cookie('Application',Application);
	}
	if(appUuid == "undefined"){
      appUuid = $.cookie('appName');
	}
	if(Application==null){
       Application = $.cookie('Application');
	}
	$(function(){
	  $('#showName').text(Application);
		if (!getToken() || getToken()==''){
			logout();
		}	
		getAppProfile(appUuid);
		showAndoid();
	});	
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
	
	// 选项卡显示
	function showAndoid(){
		$('#androidTab').addClass('active');
		$('#iosTab').removeClass('active');
		$('#iosLi').hide();
		$('#androidLi').show();	
	}
	function showIos(){
		$('#iosTab').addClass('active');
		$('#androidTab').removeClass('active');
		$('#androidLi').hide();
		$('#iosLi').show();
	}
	
	//显示修改缩略图
	function showImage(){
		$('#imageWidth').val('');
		$('#imageHeight').val('');
		$('#showUpdateImage').click();
	}
	
	//修改缩略图
	function updateImageHTML(){
		updateImage(appUuid);
	}
</script>


<div>
 <a style="display:none" href="#updateImage" id="showUpdateImage" data-toggle="modal" role="button" ></a>
<div id="main-container" class="container-fluid"> <a href="javascript:void(0);" id="menu-toggler"> <span></span> </a>
  <div id="sidebar">
    <div id="sidebar-shortcuts">
      <div style="min-height: 40px;" id="sidebar-shortcuts-large"> </div>
      <div style="min-height: 40px;" id="sidebar-shortcuts-mini"> </div>
    </div>
    <ul class="nav nav-list">
			<li> <a href="/console/app_list" target="_self"> <i class="icon-ambulance"></i> <span>我的应用</span> </a></li>
			<li> <a href="/console/admin_home" target="_self"> <i class="icon-user"></i> <span>个人信息</span> </a></li>
			<li class="active"> <a href="javascript:toApppofile()" target="_self"> <i class="icon-info-sign"></i><span>应用概况</span> </a></li>
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
        	<div class="apply" style="background:#FFF">
	      	  <table class="table table-striped table-bordered table-hover" style="width:400px;">
	      	    <tr>
	      	      <td align="right" width="150px;"><strong>应用标识(AppKey): </td>
	      	      <td width="160px;"><span id="appKey"></span></strong></td>
	      	    </tr>
	      	    <tr>
	      	      <td align="right" width="150px"><strong>创建时间: </td>
	      	      <td width="150px;"><span id="created"></span></strong></td>
	      	    </tr>
	      	    <tr>
	      	      <td align="right" width="150px"><strong>最后修改时间: </td>
	      	      <td width="150px;"><span id="modified"></span></strong></td>
	      	    </tr>
	      	    <tr>
	      	      <td align="right" width="150px"><strong>用户注册模式: </td>
	      	      <td width="150px;"><span id="allowOpen"></span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<a onClick="changeAllowOpen();" class="btn btn-mini btn-info">切换</a></strong></td>
	      	    	<input type="hidden" id="allowOpenHdd" name="allowOpenHdd" value=""/>
	      	    </tr>
	      	    <tr>
	      	      <td align="right" width="150px"><strong>client_id: </td>
	      	      <td width="150px;"><span id="client_id"></span></strong></td>
	      	    </tr>
	      	    <tr>
	      	      <td align="right" width="150px"><strong>client_secret: </td>
	      	      <td width="150px;"><span id="client_secret"></span></strong></td>
	      	    </tr>
	      	    <tr>
	      	      <td align="right" width="150px"><strong>缩略图大小: </td>
	      	      <td width="150px;"><span id="image_thumbnail_width"></span>&nbsp;&nbsp;*&nbsp;&nbsp;<span id="image_thumbnail_height"></span>(长 * 宽)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<a onClick="showImage();" class="btn btn-mini btn-info">修改</a></strong></td>
	      	    </tr>
	   	      </table>
	      	</div>
	      	<p>
	      	<div class="apply"><font size="3">快速集成</font>
	      	</div>
	      	<p/>
	      	<div class="count">
						<div class="row-fluid">
							<ul class="nav nav-tabs">
								<li id="androidTab" class="active"><a href="javascript:void(0)" onClick="showAndoid()">Android</a></li>
								<!--<li id="iosTab"><a href="javascript:void(0)" onClick="showIos()">IOS</a></li>-->
							</ul>
      				<div class="pagination pagination-left">
      					<ul class="pagination">
						      <li id="androidLi">
						      	<a href="javascript:void(0)">
										  <div>// 向AndroidManifest.xml文件中加入以下配置&ltbr/&gt</div>
											&lt!-- 环聊sdk需要的权限 --&gt
											<div>&ltuses-permission android:name="android.permission.VIBRATE" /&gt</div>
											<div>&ltuses-permission android:name="android.permission.INTERNET" /&gt</div>
											<div>&ltuses-permission android:name="android.permission.ACCESS_NETWORK_STATE" /&gt</div>
											<div>&ltuses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" /&gt</div>
											<div>&ltuses-permission android:name="android.permission.ACCESS_FINE_LOCATION" /&gt</div>
											<div>&ltuses-permission android:name="android.permission.CALL_PHONE" /&gt</div>
											<div>&ltuses-permission android:name="android.permission.GET_TASKS" /&gt</div>
											<div>&ltuses-permission android:name="android.permission.ACCESS_WIFI_STATE" /&gt</div>
											<div>&ltuses-permission android:name="android.permission.CHANGE_WIFI_STATE" /&gt</div>
											<div>&ltuses-permission android:name="android.permission.WAKE_LOCK" /&gt</div>
											<div>&ltuses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" /&gt</div>
											<div>&ltuses-permission android:name="android.permission.READ_PHONE_STATE" /&gt</div>
											&lt!-- 设置应用的 appkey --&gt
											<div>&ltmeta-data</div>
											<div>android:name="EASEMOB_APPKEY"</div>
											<div>android:value="<span id="xmlandroidAppkey"></span>" /&gt</div>	
						      	</a>
						      </li>
						      <!--<li id="iosLi">
						      	<a href="javascript:void(0)">
										  <div>// 向info.plist文件中加入以下配置</div>
								    	<div>Key:EASEMOB_APPKEY;</div>
											<div><span id="xmliosAppkey"></span></div>
											<div>Type:String;</div>
											<div>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
												&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
												&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
												&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
												&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
												&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
												&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
												&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
											<div>&nbsp;</div>
						      	</a>
						      </li>-->
				    		</ul>
      				</div>
      			</div>
	       	</div>
      </div>
    </div>
  </div>
</div>
       <!--修改缩略图大小-->
		<div id="updateImage" class="modal hide fade" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
			<div class="modal-header">
				<button id="closeButn" type="button" class="close" data-dismiss="modal" aria-hidden="true">×</button>
				<h3 id="myModalLabel">修改缩略图大小</h3>
			</div>
			<div class="modal-body">
				<div class="row-fluid">
					<div class="span12">
						<div class="control-group">
							<label class="control-label" for="imageHeight">长：</label>
							<div class="controls" id="div1">
								<input type="text" id="imageHeight" name="imageHeight" value=""/>
								<input type="hidden" id="usernameMessage" name="usernameMessage" value=""/>
								<input type="hidden" id="appUuidMessage" name="appUuidMessage" value=""/>
							</div>
						</div>
					</div>
				</div>
			</div>
			<div class="modal-body">
				<div class="row-fluid">
					<div class="span12">
						<div class="control-group">
							<label class="control-label" for="imageWidth">宽：</label>
							<div class="controls" id="div1">
								<input type="text" id="imageWidth" name="imageWidth" value=""/>
							</div>
						</div>
					</div>
				</div>
			<div class="modal-footer"> 
				<a class="btn" onClick="updateImageHTML();" href="javascript:void(0);">确定</a>
				<button id="messageCloseBtn" class="btn" data-dismiss="modal" onClick="clsSpan()" aria-hidden="true">关闭</button>
			</div>
		</div>
        <!--修改缩略图大小end-->
</div>

