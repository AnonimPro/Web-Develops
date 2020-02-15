# Web Develops

!!! Example of code
!!! Language: PHP
``` php
<?php

namespace base\controllers;

class Controller{
	private $_variables;
	public function __construct(){
		$this->_variables = [];
	}

	public function loadHelper($titleHelper){
		$pathCoreHelper = 'core/helpers/'.$titleHelper.'.php';
		$pathAppHelper = 'application/helpers/'. $titleHelper .'.php';
		if (file_exists($pathCoreHelper)){
			require_once($pathCoreHelper);
			return true;
		} elseif (file_exists($pathAppHelper)){
			require_once($pathAppHelper);
			return true;
		}
		return false;
	}
	
	public function loadModel($titleModel, $aliasModel){
		$titleModule = $this->titleModule();
		$pathModel = '';
		$modelTitle = '';
		
		if ($titleModule !== false){
			$pathModel = 'application/modules/'.$titleModule.'/models/'.$titleModel.'.php';
			$modelTitle = '\\base\\'.$titleModule.'\\models\\'.$titleModel;
			if (file_exists($pathModel)){
				require_once($pathModel);
				$this->$aliasModel = new $modelTitle;
				return true;
			}
		}
		
		$pathModel = 'application/models/'.$titleModel.'.php';
		$modelTitle = '\\base\models\\'.$titleModel;
		
		if (file_exists($pathModel)){
			require_once($pathModel);
			$this->$aliasModel = new $modelTitle;
			return true;
		}
		
		return false;
	}
	
	public function loadLibrary($titleLibrary, $aliasLibrary){
		$pathLibrary = 'application/libraries/'.$titleLibrary.'.php';
		if (file_exists($pathLibrary)){
			require_once($pathLibrary);
			$titleLibrary = '\\base\libraries\\'.$titleLibrary;
			$this->$aliasLibrary = new $titleLibrary;
			
			return true;
		}
		return false;
	}
	
	public function data($titleVariable, $valueVariable){
		$this->_variables[] = [
			'title' => $titleVariable,
			'value' => $valueVariable
		];
	}

	public function display($titleView){
		$titleModule = $this->titleModule();
		
		for($i=0;$i<count($this->_variables);$i++){
			${$this->_variables[$i]['title']} = $this->_variables[$i]['value'];
		}
		
		if ($titleModule !== false){
			$pathView = 'application/modules/'.$titleModule.'/views/'.$titleView.'.php';
			if (file_exists($pathView)){
				require_once($pathView);
				return true;
			}
		}
		$pathView = 'application/views/'.$titleView.'.php';

		if (file_exists($pathView)){
			require_once($pathView);
			return true;
		}

		return false;
	}
}
```
