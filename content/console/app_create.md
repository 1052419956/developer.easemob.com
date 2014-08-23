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
<script src="/assets/js/jquery.cookie-1.3.js"></script>
<script src="/assets/js/bootstrap-2.3.2.min.js"></script>
<script src="/assets/js/json2.js"></script>
<script src="/assets/js/ace-elements.min.js"></script>
<script src="/assets/js/ace.min.js"></script>
<script src="/assets/js/management.js"></script>
<script tyep="text/javascript">
	$(function(){
		if (!getToken() || getToken()==''){
			logout();
		}
		
		if($('#allow_open_registration1').attr('checked') == 'checked'){
			$('#allowOpenMsg').text('允许在该应用下自由注册新用户');
		}
		if($('#allow_open_registration2').attr('checked') == 'checked'){
			$('#allowOpenMsg').text('只有企业管理员或者应用管理员才能注册用户');
		}
		
		$('#allow_open_registration1').click(function(){
			$('#allowOpenMsg').text('允许在该应用下自由注册新用户');
		});
		$('#allow_open_registration2').click(function(){
			$('#allowOpenMsg').text('只有企业管理员或者应用管理员才能注册用户');
		});
	});	
	
	function removeAllSpace(str){
		return str.replace(/\s+/g,'');	
	}
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
            <div class="widget-body"></div>
            <tbody id="appListBody">
            	<div class="widget-body">
			            <div class="widget-main no-padding">
			              <div class="form-horizontal" style="padding-top:20px;">
			              	<!--<div class="row-fluid">
			                  <div class="span12">
			                    <div class="control-group">
			                      <label for="appName" class="control-label">应用ID：</label>
			                      <div class="controls">
			                      	<input type="text" id="appID" name="appID" value="" />
			                      </div>
			                    </div>
			                  </div>
			                </div>
			                -->
											<div class="row-fluid">
			                  <div class="span12">
			                    <div class="control-group">
			                      <label for="appName" class="control-label">应用名称：</label>
			                      <div class="controls">
			                      	<input type="text" id="appName" name="appName" value="" onKeyUp="value=removeAllSpace(value)" onBlur="value=removeAllSpace(value)" />
			                      	<span style="color:red">*&nbsp;</span>
			                      	<span id="appNameMsg">应用名称只能是字母,数字或字母数字组合!</span>
			                      </div>
			                    </div>
			                  </div>
			                </div>
			                <div class="row-fluid">
			                  <div class="span12">
			                    <div class="control-group">
			                      <label for="appName" class="control-label">产品名称：</label>
			                      <div class="controls">
			                      	<input type="text" id="nick" name="nick" value="" onKeyUp="value=removeAllSpace(value)" onBlur="value=removeAllSpace(value)" />
			                      	<span style="color:red">*&nbsp;</span>
			                      	<span id="nickMsg">产品名称只能是汉字,字母,数字、横线、下划线及其组合!</span>
			                      </div>
			                    </div>
			                  </div>
			                </div>
			                <div class="row-fluid">
			                  <div class="span12">
			                    <div class="control-group">
			                      <label class="control-label">注册模式：</label>
			                      <div class="controls" style="padding-top:5px;">
			                      	<input type="radio" id="allow_open_registration1" name="allow_open_registration" value="true" /><span class="lbl">开放注册</span>
			                      	<input type="radio" id="allow_open_registration2" name="allow_open_registration" value="false" checked="checked"/><span class="lbl">授权注册</span>
			                      	<span style="color:red">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*</span>
			                      	<span id="allowOpenMsg">&nbsp;</span>
			                      </div>
			                    </div>
			                  </div>
			                </div>
			              	<div class="row-fluid">
			                  <div class="span12">
			                    <div class="control-group">
			                      <label for="appDesc" class="control-label">应用描述：</label>
			                      <div class="controls">
			                        <textarea rows="7" cols="12" id="appDesc" name="appDesc" onKeyUp="value=removeAllSpace(value)" onBlur="value=removeAllSpace(value)"></textarea>
			                      	<span id="appDescMsg">应用描述只能输入字母，数字或者汉字,字数在一百字以内!</span>
			                        </div>
			                    </div>
			                  </div>
			                </div>
			                <div class="form-actions">
							  				<button onClick="toAppList();" href="javascript:void(0);" class="btn btn-small btn-success"><i class="icon-arrow-left icon-on-right bigger-110"></i> 返回列表 </button>
			                  <button onClick="saveNewApp();" href="javascript:void(0);" class="btn btn-small btn-success"> 确定<i class="icon-arrow-right icon-on-right bigger-110"></i> </button>
			                </div>
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