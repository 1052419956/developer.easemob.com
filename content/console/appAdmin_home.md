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


<link href="/assets/css/management.css" rel="stylesheet" type="text/css" media="screen"/>
<link href="/assets/css/manage.css" rel="stylesheet" type="text/css" media="screen"/>


<script src="/assets/js/jquery-1.7.2.min.js"></script>
<script src="/assets/js/jquery.cookie-1.3.js"></script>
<script src="/assets/js/bootstrap-2.3.2.min.js"></script>
<script src="/assets/js/json2.js"></script>
<script src="/assets/js/ace-elements.min.js"></script>
<script src="/assets/js/ace.min.js"></script>
<script src="/assets/js/management.js"></script>
<script type="text/javascript">
	$(function(){
		if (!getToken() || getToken()==''){
			logout();
		}
		adminInfo();
	});
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
			<li class="active"> <a href="/console/admin_home" target="_self"> <i class="icon-user"></i> <span>个人信息</span> </a></li>
    </ul>
    <div id="sidebar-collapse"> <i class="icon-double-angle-left"></i> </div>
  </div>
  <div class="clearfix" id="main-content">
    <div id="breadcrumbs">
      <ul class="breadcrumb">
        <li> <i class="icon-home"></i>  个人信息 </li>
      </ul>
    </div>
    <div class="clearfix" id="page-content">
    	<div class="pagination pagination-left">
	  		<ul>
	    		<li> <a href="/console/admin_home" ><font color="green">个人信息</font></a> </li>
	  		</ul>
	  		&nbsp;&nbsp;&nbsp;&nbsp;
	  		<ul>
      		<li> <a href="/console/admin_home_passwd">修改密码</a> </li>
    		</ul>
			</div>
      <div class="row-fluid">
        <div class="row-fluid">
          <table class="table table-striped table-bordered table-hover">
          	<div class="widget-body"></div>
            <tbody id="appListBody">
            	<div class="widget-body">
			            <div class="widget-main no-padding">
			              <div class="form-horizontal" style="padding-top:20px;">
											<div class="row-fluid">
			                  <div class="span12">
			                    <div class="control-group">
			                      <label for="username" class="control-label">登录账号：</label>
			                      <div class="controls" style="padding-top:5px;">
			                      		<span id="username"></span>
			                      </div>
			                    </div>
			                  </div>
			                </div>
			                <div class="row-fluid">
			                  <div class="span12">
			                    <div class="control-group">
			                      <label for="email" class="control-label">邮箱账号：</label>
			                      <div class="controls" style="padding-top:5px;">
			                       	<span id="email"></span>
			                      </div>
			                    </div>
			                  </div>
			                </div>
			                 <div class="row-fluid">
			                  <!--<div class="span12">
			                    <div class="control-group">
			                      <label for="createTime" class="control-label">创建日期：</label>
			                      <div class="controls" style="padding-top:5px;">
			                      	<span id="createTime"></span>
			                      </div>
			                    </div>
			                  </div>
			                </div>-->
						  			</div>
			            </div>
			          </div>	
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</div>


</div>
