<?php
class Memento{
	private $state;
	
	function __construct($state){
		$this->state=$state;
	}
	
	function getState(){
		return $this->state;
	}
}

class Originator{
	private $state;
	
	function setState($state){
		$this->state=$state;
	}
	
	function getState(){
		return $this->state;
	}
	
	function save2memento(){
		//Save state set to create a Memento object
		return new Memento($this->state);
	}
	
	function restoreMemento($memento){
		$this->state=$memento->getState();
	}
}

//To comply with the Dimitt principle, add a class for managing memos.
class CareTaker{	
	private $mementoList=[];
	
	function addMemento($mementoState){
		array_push($this->mementoList,$mementoState);
	}
	
	function pullMemento($step=1){
		if(sizeof($this->mementoList)>$step){
			$ret_memento='';
			for($i=0;$i<$step;$i++){
				array_pop($this->mementoList);
			}

			return $this->mementoList[sizeof($this->mementoList)-1];
		}

		//Return to initial state
		return $this->mementoList[0];
	}
}

function main(){

	$originator = new Originator;
	$careTaker = new CareTaker;

	$originator->setState('state #1');
	echo "Current State: ".$originator->getState(),PHP_EOL; 

	//Procedure: Originator sets state, saves state to Memento,
	//Join Memento to the CareTaker queue
	$careTaker->addMemento($originator->save2memento());														

	$originator->setState('state #2');
	echo "Current State: ".$originator->getState(),PHP_EOL; 
	$careTaker->addMemento($originator->save2memento());

	$originator->setState('state #3');
	echo "Current State: ".$originator->getState(),PHP_EOL; 
	$careTaker->addMemento($originator->save2memento());

	//Get the Memento object from CareTaker and put the state of the Memento object
	//Assigned to Originator
	// $originator->restoreMemento($careTaker->pullMemento());														
	$originator->restoreMemento($careTaker->pullMemento(2));														
	echo "Current State: ".$originator->getState(),PHP_EOL; 

	$originator->setState('state #4');
	echo "Current State: ".$originator->getState(),PHP_EOL; 
	$careTaker->addMemento($originator->save2memento());

}main();

?>
