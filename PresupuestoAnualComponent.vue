<template>
    <div>
        <div id="main" v-show="vistainfo == false">
            <div class="row">
                <div class="content-wrapper-before gradient-45deg-green-teal"></div>
                <div class="breadcrumbs-dark pb-0 pt-4" id="breadcrumbs-wrapper">
                    <!-- Search for small screen-->
                    <div class="container">
                        <div class="row">
                            <div class="col s8 m8 l9">
                                <h5 class="breadcrumbs-title mt-0 mb-0"><span>Presupuesto Anual</span></h5>
                                <ol class="breadcrumbs mb-0">
                                    <li class="breadcrumb-item"><a href="#">Inicio</a>
                                    </li>
                                    <li class="breadcrumb-item active">Presupuesto Anual
                                    </li>
                                </ol>
                            </div>
                            <div class="col s4 m4 l3">
                                <a class="btn waves-effect waves-light breadcrumbs-btn right modal-trigger red darken-4 border-round"
                                    href="#modal1" @click.prevent="infoPresupuesto()">
                                    <i class="material-icons hide-on-med-and-up">add</i>
                                    <span class="hide-on-small-onl">Nuevo</span><i
                                        class="material-icons right">add</i></a>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col s12">
                    <div class="container">
                        <div class="section">
                            <div class="card">
                                <div class="card-content animate fadeLeft">
                                    <div class="row">
                                        <form class="col s12">
                                            <div class="row">
                                                <div class="input-field col s0 m3 l3">
                                                </div>
                                                <div class="input-field col s12 m6 l6">
                                                    <i class="material-icons prefix">search</i>
                                                    <input id="icon_prefix10" type="text" class="validate"
                                                        @keyup="buscarPresupuesto" v-model="searchpresupuesto">
                                                    <label for="icon_prefix10">Buscar</label>
                                                </div>
                                                <div class="input-field col s0 m3 l3">
                                                </div>
                                            </div>
                                        </form>
                                    </div>
                                </div>
                            </div>
                        </div>
                         <!-- seccion que se le muestra al perfil de contabilidad -->
                        <div class="section users-edit" v-if="id_usuario_logeado == usuario_contabilidad">
                        <div class="card">
                            <div class="card-content">
                            <!-- <div class="card-body"> -->
                            <ul class="tabs mb-2 row">
                                <li class="tab">
                                <a class="display-flex align-items-center active" id="account-tab" href="#account">
                                    <i class="material-icons mr-1">person_outline</i><span>Mis Presupuestos</span>
                                </a>
                                </li>
                                <li class="tab">
                                <a class="display-flex align-items-center" id="information-tab" href="#information">
                                    <i class="material-icons mr-2">error_outline</i><span>Presupuestos en revisión</span>
                                </a>
                                </li>
                            </ul>
                            <div class="divider mb-3"></div>
                            <div class="row">
                                <div class="col s12" id="account">
                                    <div class="card-content animate fadeRight">
                                        <div class="row">
                                            <div class="col s12">
                                                <table class="table responsive-table highlight">
                                                    <thead>
                                                        <tr>
                                                            <th>#</th>
                                                            <th>Folio</th>
                                                            <th>Solicitante</th>
                                                            <th>Area</th>
                                                            <th>Fecha</th>
                                                            <th>Ejercicio</th>
                                                            <th>Estatus</th>
                                                            <th></th>
                                                        </tr>
                                                    </thead>
                                                    <tbody>
                                                        <tr v-for="(presupuesto, index) in presupuestos" :key='index' v-if="presupuesto.solicitante == name_usuario_logeado">
                                                            <td>{{ presupuesto.id_pres}}</td>
                                                            <td>{{ presupuesto.folio }}</td>
                                                            <td>{{ presupuesto.solicitante }}</td>
                                                            <td>{{ presupuesto.area }}</td>
                                                            <td>{{ presupuesto.fecha }}</td>
                                                            <td>{{ presupuesto.eje }}</td>
                                                            <td><span
                                                                    :class="`badge lighten-5 ${presupuesto.estatus_class} text-accent-4`"
                                                                    style="border-radius:10px;">{{ presupuesto.estatus_nuevo}}</span>
                                                            </td>
                                                                <td>
                                                                <a class="btn-floating waves-effect waves-light btn-small accent-4 yellow modal-trigger"
                                                                    href="#" @click.prevent="infoPresupuestoDetalle(presupuesto),listadoConceptos(presupuesto),cambiar(true)"><i
                                                                        class="material-icons">remove_red_eye</i></a>
                                                            <b v-if="presupuesto.estatus_nuevo === 'Aprobada' || presupuesto.estatus_nuevo === 'Rechazada' || presupuesto.estatus_nuevo === 'En revisión' || presupuesto.estatus_nuevo === 'Por aprobar'">
                                                                    </b>
                                                                    <b v-else>
                                                                            <a class="btn-floating waves-effect waves-light btn-small accent-4 blue modal-trigger"
                                                                        href="#modaleditarpresupuesto" 
                                                                        @click.prevent="listadoConceptos(presupuesto)"><i
                                                                            class="material-icons">edit</i></a>
                                                                    <a
                                                                        class="btn btn-floating red waves-effect waves-deep-orange accent-4 btn-small"><i
                                                                            class="material-icons">delete</i></a>
                                                                    </b>
                                                            </td>
                                                        </tr>
                                                    </tbody>
                                                </table>
                                                <div class="row">
                                                    <center>
                                                        <ul class="pagination mt-3">
                                                            <li class="waves-effect" v-if="pagination.current_page > 1"><a
                                                                    href="#!"
                                                                    @click.prevent="changePage(pagination.current_page -1)"><i
                                                                        class="material-icons">chevron_left</i></a>
                                                            </li>
                                                            <li class="waves-effect" v-for="(page, index) in pageNumber"
                                                                :key="index" @click.prevent="changePage(page)"
                                                                v-bind:class="[ page == isActived ? 'active' : 'waves-effect']">
                                                                <a href="#!">{{page}}</a></li>
                                                            <li class="waves-effect"
                                                                v-if="pagination.current_page < pagination.last_page">
                                                                <a href="#!"
                                                                    @click.prevent="changePage(pagination.current_page + 1)"><i
                                                                        class="material-icons">chevron_right</i></a>
                                                            </li>
                                                        </ul>
                                                    </center>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="col s12" id="information">
                                    <div class="card-content animate fadeRight">
                                        <div class="row">
                                            <div class="col s12">
                                                <table class="table responsive-table highlight">
                                                    <thead>
                                                        <tr>
                                                            <th>#</th>
                                                            <th>Folio</th>
                                                            <th>Solicitante</th>
                                                            <th>Area</th>
                                                            <th>Fecha</th>
                                                            <th>Ejercicio</th>
                                                            <th>Estatus</th>
                                                            <th></th>
                                                        </tr>
                                                    </thead>
                                                    <tbody>                                   
                                                        <tr v-for="(presupuesto, index) in presupuestos" :key='index' v-if="presupuesto.estatus_nuevo === 'En revisión'">
                                                            <td>{{ presupuesto.id_pres}}</td>
                                                            <td>{{ presupuesto.folio }}</td>
                                                            <td>{{ presupuesto.solicitante }}</td>
                                                            <td>{{ presupuesto.area }}</td>
                                                            <td>{{ presupuesto.fecha }}</td>
                                                            <td>{{ presupuesto.eje }}</td>
                                                            <td><span
                                                                    :class="`badge lighten-5 ${presupuesto.estatus_class} text-accent-4`"
                                                                    style="border-radius:10px;">{{ presupuesto.estatus_nuevo}}</span>
                                                            </td>
                                                                <td>
                                                                <a class="btn-floating waves-effect waves-light btn-small accent-4 yellow modal-trigger"
                                                                    href="#"
                                                                    @click.prevent="infoPresupuestoDetalle(presupuesto),listadoConceptos(presupuesto),cambiar(true)"><i
                                                                        class="material-icons">remove_red_eye</i></a>
                                                                    <b v-if="presupuesto.estatus_nuevo === 'Aprobada' || presupuesto.estatus_nuevo === 'Rechazada' || presupuesto.estatus_nuevo === 'En revisión' || presupuesto.estatus_nuevo === 'Por aprobar'">
                                                                    </b>
                                                                    <b v-else>
                                                                            <a class="btn-floating waves-effect waves-light btn-small accent-4 blue modal-trigger"
                                                                        href="#modaleditarpresupuesto" 
                                                                        @click.prevent="listadoConceptos(presupuesto)"><i
                                                                            class="material-icons">edit</i></a>
                                                                    <a
                                                                        class="btn btn-floating red waves-effect waves-deep-orange accent-4 btn-small"><i
                                                                            class="material-icons">delete</i></a>
                                                                    </b>
                                                            </td>
                                                        </tr>
                                                    </tbody>
                                                </table>
                                                <div class="row">
                                                    <center>
                                                        <ul class="pagination mt-3">
                                                            <li class="waves-effect" v-if="pagination.current_page > 1"><a
                                                                    href="#!"
                                                                    @click.prevent="changePage(pagination.current_page -1)"><i
                                                                        class="material-icons">chevron_left</i></a>
                                                            </li>
                                                            <li class="waves-effect" v-for="(page, index) in pageNumber"
                                                                :key="index" @click.prevent="changePage(page)"
                                                                v-bind:class="[ page == isActived ? 'active' : 'waves-effect']">
                                                                <a href="#!">{{page}}</a></li>
                                                            <li class="waves-effect"
                                                                v-if="pagination.current_page < pagination.last_page">
                                                                <a href="#!"
                                                                    @click.prevent="changePage(pagination.current_page + 1)"><i
                                                                        class="material-icons">chevron_right</i></a>
                                                            </li>
                                                        </ul>
                                                    </center>
                                                </div>
                                            </div>

                                        </div>
                                    </div>
                                </div>
                            </div>
                            <!-- </div> -->
                            </div>
                        </div>
                        </div>
                        <!-- cierra seccion que se le muestra al perfil de contabilidad -->
                        <div class="section" v-else>
                            <!-- Responsive Table -->
                                        <div class="row">
                                            <div class="col s12 m12 l12">
                                                <div id="responsive-table" class="card card-default scrollspy">
                                                    <div class="card-content animate fadeRight">
                                                        <div class="row">
                                                            <div class="col s12" v-if="presupuestos.length == 0">
                                                                <div class="card-alert card red">
                                                                    <div class="card-content white-text center">
                                                                    <p>No tienes presupuestos</p>
                                                                    </div>
                                                                </div>
                                                            </div>
                                                            <div class="col s12" v-else>
                                                                <table class="table responsive-table highlight">
                                                                    <thead>
                                                                        <tr>
                                                                            <th>#</th>
                                                                            <th>Folio</th>
                                                                            <th>Solicitante</th>
                                                                            <th>Area</th>
                                                                            <th>Fecha</th>
                                                                            <th>Ejercicio</th>
                                                                            <th>Estatus</th>
                                                                            <th></th>

                                                                        </tr>
                                                                    </thead>
                                                                    <tbody>
                                                                        <tr v-for="(presupuesto, index) in presupuestos" :key='index'>
                                                                            <td>{{ presupuesto.id_pres}}</td>
                                                                            <td>{{ presupuesto.folio }}</td>
                                                                            <td>{{ presupuesto.solicitante }}</td>
                                                                            <td>{{ presupuesto.area }}</td>
                                                                            <td>{{ presupuesto.fecha }}</td>
                                                                            <td>{{ presupuesto.eje }}</td>
                                                                            <td><span
                                                                                    :class="`badge lighten-5 ${presupuesto.estatus_class} text-accent-4`"
                                                                                    style="border-radius:10px;">{{ presupuesto.estatus_nuevo}}</span>
                                                                            </td>
                                                                            <td>

                                                                                <a class="btn-floating waves-effect waves-light btn-small accent-4 yellow modal-trigger"
                                                                                    href="#"
                                                                                    @click.prevent="infoPresupuestoDetalle(presupuesto),listadoConceptos(presupuesto),cambiar(true)"><i
                                                                                        class="material-icons">remove_red_eye</i></a>
                                                                                    <b v-if="rol === 'Contabilidad' || presupuesto.estatus_nuevo === 'Aprobada' || presupuesto.estatus_nuevo === 'Rechazada' || presupuesto.estatus_nuevo === 'En revisión' || presupuesto.estatus_nuevo === 'Por aprobar'">
                                                                                    </b>
                                                                                    <b v-else>
                                                                                        <a class="btn-floating waves-effect waves-light btn-small accent-4 blue modal-trigger"
                                                                                        href="#modaleditarpresupuesto" 
                                                                                        @click.prevent="listadoConceptos(presupuesto)"><i
                                                                                            class="material-icons">edit</i></a>
                                                                                    <a
                                                                                        class="btn btn-floating red waves-effect waves-deep-orange accent-4 btn-small"><i
                                                                                            class="material-icons">delete</i></a>
                                                                                    </b>
                                                                            </td>

                                                                        </tr>
                                                                    </tbody>
                                                                </table>
                                                                <div class="row">
                                                                    <center>
                                                                        <ul class="pagination mt-3">
                                                                            <li class="waves-effect" v-if="pagination.current_page > 1"><a
                                                                                    href="#!"
                                                                                    @click.prevent="changePage(pagination.current_page -1)"><i
                                                                                        class="material-icons">chevron_left</i></a>
                                                                            </li>
                                                                            <li class="waves-effect" v-for="(page, index) in pageNumber"
                                                                                :key="index" @click.prevent="changePage(page)"
                                                                                v-bind:class="[ page == isActived ? 'active' : 'waves-effect']">
                                                                                <a href="#!">{{page}}</a></li>
                                                                            <li class="waves-effect"
                                                                                v-if="pagination.current_page < pagination.last_page">
                                                                                <a href="#!"
                                                                                    @click.prevent="changePage(pagination.current_page + 1)"><i
                                                                                        class="material-icons">chevron_right</i></a>
                                                                            </li>
                                                                        </ul>
                                                                    </center>
                                                                </div>
                                                            </div>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                        </div>
                        <!-- users edit ends -->
                    </div>
                </div>
            </div>
            <br><br><br>
           <notificacionesComponent ref="notificacionesComponente"></notificacionesComponent>
            
        </div>
        <div id="main" v-show="vistainfo">
            <div class="row">
        <div class="content-wrapper-before "></div>
        <div class="col s12">
          <div class="container">
            <!-- app invoice View Page -->
<section class="invoice-view-wrapper section">
  <div class="row">
    <!-- invoice view page -->
    <div class="col xl10 m8 s12">
      <div class="card">
        <div class="card-content invoice-print-area">
          <!-- header section -->
          <div class="row invoice-date-number">
            <div class="col xl4 s12">
              <span class="invoice-number mr-1">Ejercicio:</span>
              <span>{{ informacionDetallePresupuesto.eje}}</span>
            </div>
            <div class="col xl8 s12">
              <div class="invoice-date display-flex align-items-center flex-wrap">
                <div>
                  <small>Fecha:</small>
                  <span>{{informacionDetallePresupuesto.fecha}}</span>
                </div>
              </div>
            </div>
          </div>
         <div class="col s12 m6 xl4">
                            <div id="profile-card" class="card">
                                <div class="card-content">
                                    <h5 class="card-title activator grey-text text-darken-4">De:
                                        {{informacionDetallePresupuesto.solicitante}}</h5>
                                    <p><i class="material-icons profile-card-i">perm_identity</i>
                                        {{informacionDetallePresupuesto.area}}</p>
                                    <p><i class="material-icons profile-card-i">perm_phone_msg</i>
                                        {{usuarioSolicitante.telefono}}</p>
                                    <p><i class="material-icons profile-card-i">email</i>{{usuarioSolicitante.email}}
                                    </p>
                                </div>
                            </div>
                        </div>
                        <div class="col s12 m6 xl4">
                            <div id="profile-card" class="card">
                                <div class="card-content">
                                    <h5 class="card-title activator grey-text text-darken-4">Para:
                                        {{ usuarioPara.name }}</h5>
                                    <p><i class="material-icons profile-card-i">perm_identity</i> {{ usuarioPara.rol }}
                                    </p>
                                    <p><i class="material-icons profile-card-i">perm_phone_msg</i>
                                        {{ usuarioPara.telefono }}</p>
                                    <p><i class="material-icons profile-card-i">email</i>{{ usuarioPara.email }}</p>
                                </div>
                            </div>
                        </div>
                        <div class="col s12 m12 xl4">
                            <ul id="projects-collection" class="collection z-depth-4">
                                <li class="collection-item avatar">
                                    <i class="material-icons deep-purple darken-3 circle">assignment</i>
                                    <h6 class="collection-header m-0">No.Folio:</h6>
                                    <p>{{informacionDetallePresupuesto.folio}}</p>
                                </li>
                                <li class="collection-item" style="min-height: 66px;padding: 8px 20px;">
                                    <div class="row">
                                        <div class="col s3 m3 l3">
                                            <p class="collections-title font-weight-600">Estatus:</p>
                                        </div>
                                        <div class="col s9 m9 l9">
                                            <div v-if="rol == 'Contabilidad'">
                                                 <div v-if="informacionDetallePresupuesto.estatus_nuevo == 'Aprobada'">
                                                    <span
                                                        :class="`badge lighten-5 ${informacionDetallePresupuesto.estatus_class} text-accent-4`"
                                                        style="border-radius:10px;margin-top:14px;">{{ informacionDetallePresupuesto.estatus_nuevo}}</span>
                                                </div>
                                                <div class="input-field" v-else-if="informacionDetallePresupuesto.estatus_nuevo === 'En proceso'">
                                                     <div class="row">
                                                        <div class="col l9 m9 s9">
                                                            <select v-model="estatuspresupuesto" class="select2 browser-default listadoclase">
                                                                <option v-bind:value="informacionDetallePresupuesto.estatus">
                                                                    {{informacionDetallePresupuesto.estatus_nuevo}}</option>
                                                                <option v-bind:value="1">En revisión</option>
                                                            </select>                                   
                                                        </div>
                                                        <div class="col l3 m3 s3">
                                                            <a class="mb-6 btn-floating btn-small waves-effect waves-light green darken-1" @click="estatusRevision(informacionDetallePresupuesto.id_pres)"><i class="material-icons">send</i></a>
                                                        </div>
                                                    </div>
                                                </div>
                                                <div class="input-field" v-else>
                                                    <div class="row">
                                                        <div class="col l9 m9 s9">
                                                            <select v-model="estatuspresupuesto" class="select2 browser-default listadoclase">
                                                                <option v-bind:value="informacionDetallePresupuesto.estatus">
                                                                    {{informacionDetallePresupuesto.estatus_nuevo}}</option>
                                                                <option v-bind:value="4">Por aprobar</option>
                                                                <option v-bind:value="3">Rechazar</option>
                                                            </select>
                                                        </div>
                                                        <div class="col l3 m3 s3">
                                                            <a class="mb-6 btn-floating btn-small waves-effect waves-light green darken-1" @click="editarStatusAprobadaRechazada(informacionDetallePresupuesto.id_pres)"><i class="material-icons">send</i></a>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                            <div v-else-if="rol == 'Secretaria General'">
                                                <div class="input-field">
                                                    <div class="row">
                                                        <div class="col l9 m9 s9">
                                                            <select v-model="estatuspresupuesto" class="select2 browser-default listadoclase">
                                                                <option v-bind:value="informacionDetallePresupuesto.estatus">
                                                                    {{informacionDetallePresupuesto.estatus_nuevo}}</option>
                                                                <option v-bind:value="2">Aprobada</option>
                                                                <option v-bind:value="3">Rechazada</option>
                                                            </select>                                                                                            
                                                        </div>
                                                        <div class="col l3 m3 s3">
                                                            <a class="mb-6 btn-floating btn-small waves-effect waves-light green darken-1" @click="editarStatusPorAprobar(informacionDetallePresupuesto.id_pres)"><i class="material-icons">send</i></a>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                            <div v-else>
                                                <div
                                                    v-if="informacionDetallePresupuesto.estatus_nuevo == 'En revisión' || informacionDetallePresupuesto.estatus_nuevo == 'Aprobada'">
                                                    <span
                                                        :class="`badge lighten-5 ${informacionDetallePresupuesto.estatus_class} text-accent-4`"
                                                        style="border-radius:10px;margin-top:14px;">{{ informacionDetallePresupuesto.estatus_nuevo}}</span>
                                                </div>
                                                <div
                                                    v-else-if="informacionDetallePresupuesto.estatus_nuevo == 'Rechazada'">
                                                    <form>
                                                        <div class="input-field">
                                                            <div class="row">
                                                                <div class="col l9 m9 s9">
                                                                   <select v-model="estatuspresupuesto" class="select2 browser-default listadoclase">
                                                                        <option
                                                                            v-bind:value="informacionDetallePresupuesto.estatus">
                                                                            {{informacionDetallePresupuesto.estatus_nuevo}}
                                                                        </option>
                                                                        <option v-bind:value="0">
                                                                            En Proceso</option>
                                                                    </select>                                                                                         
                                                                </div>
                                                                <div class="col l3 m3 s3">
                                                                    <a class="mb-6 btn-floating btn-small waves-effect waves-light green darken-1" @click="editarStatus(informacionDetallePresupuesto.id_pres)"><i class="material-icons">send</i></a>
                                                                </div>
                                                            </div>
                                                        </div>
                                                    </form>
                                                </div>
                                                <div v-else>
                                                    <form>
                                                        <div class="input-field">
                                                            <div class="row">
                                                                <div class="col l9 m9 s9">
                                                                    <select v-model="estatuspresupuesto" class="select2 browser-default listadoclase">
                                                                        <option
                                                                            v-bind:value="informacionDetallePresupuesto.estatus">
                                                                            {{informacionDetallePresupuesto.estatus_nuevo}}
                                                                        </option>
                                                                        <option v-bind:value="1"
                                                                            v-if="informacionDetallePresupuesto.estatus_nuevo == 'En proceso'">
                                                                            En revisión</option>
                                                                    </select>                                                                                    
                                                                </div>
                                                                <div class="col l3 m3 s3">
                                                                    <a class="mb-6 btn-floating btn-small waves-effect waves-light green darken-1" @click="estatusRevision(informacionDetallePresupuesto.id_pres)"><i class="material-icons">send</i></a>
                                                                </div>
                                                            </div>
                                                        </div>
                                                    </form>
                                                </div>
                                            </div>
                                        </div>
                                        
                                    </div>
                                </li>
                                <li class="collection-item" style="min-height: 66px;padding:5px 20px !important;">
                                    <div class="row">
                                        <div class="col s4">
                                            <p class="collections-title font-weight-600">Descripción:</p>
                                        </div>
                                        <div class="col s8">
                                            <p class="collections-title font-weight-600">
                                                {{informacionDetallePresupuesto.notas}}</p>
                                        </div>
                                    </div>
                                </li>
                                <li class="collection-item" style="min-height: 66px;padding: 5px 20px !important;">
                                    <div class="row">
                                        <div class="col s3">
                                            <p class="collections-title font-weight-600">Notas:</p>
                                        </div>
                                        <div class="col s9">
                                            <a class="btn-floating waves-effect waves-light btn-small accent-4 yellow modal-trigger"
                                                href="#modallistadonotas"
                                                @click.prevent="listaNotas(informacionDetallePresupuesto.id_pres)"><i
                                                    class="material-icons">remove_red_eye</i></a>
                                            <a class="btn-floating waves-effect waves-light btn-small accent-4 blue modal-trigger"
                                                v-if="rol == 'Contabilidad' || rol == 'Secretaria General'" href="#modalnotas"
                                                @click.prevent="asignarIdPresupuesto(informacionDetallePresupuesto.id_pres)"><i
                                                    class="material-icons">add_circle_outline</i></a>
                                        </div>
                                    </div>
                                </li>
                            </ul>
                        </div>
                        <table class="table highlight">
                            <thead>
                                <tr>
                                    <th>Concepto</th>
                                    <th>Periodo pago</th>
                                    <th>Cantidad</th>
                                    <th>Precio Unitario</th>
                                    <th>Precio Total</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(lc, index) in conceptoListado" :key="index">
                                    <td>{{ lc.concepto }}</td>
                                    <td>{{ lc.periodo }}</td>
                                    <td>{{ lc.cantidad }}</td>
                                    <td>$ {{ formatPrice(lc.precioUnitario) }}</td>
                                    <td>$ {{ formatPrice(lc.precioTotal) }}</td>
                                </tr>
                            </tbody>
                        </table>
                        <br><br>
                         <div class="col s12 m6 l6" style="text-align: center;">
                            Total Precio Unitario: $ {{ formatPrice(informacionDetallePresupuesto.subtotal_final) }}
                        </div>
                        <div class="col s12 m6 l6" style="text-align: center;">
                            Total: $ {{ formatPrice(informacionDetallePresupuesto.total_final) }}
                        </div>
                        <br><br><br>
                                   <!-- abre modal de alta de nota -->
            <div id="modalnotas" class="modal modal-fixed-footer" style="height:40%;width:40%;">
                <div class="modal-content">
                    <h4>Agregar Nota</h4>
                    <br><br>
                    <div class="input-field col l12 s12 m12">
                        <input value="Alvin" id="first_name2" v-model="notas.descripcion" type="text" class="validate">
                        <label class="active" for="first_name2">Descripción</label>
                    </div>
                </div>
                <div class="modal-footer">
                    <button class="btn waves-effect waves-light gradient-45deg-green-teal border-round"
                        @click.prevent="agregarNota(presupuesto)" type="button" name="action">Guardar
                        <i class="material-icons right">send</i>
                    </button>
                </div>
            </div>
            <!-- cierra modal de alta de nota -->
            <!-- abre modal de listado de notas -->
            <!-- Modal Structure -->
            <div id="modallistadonotas" class="modal bottom-sheet">
                <div class="modal-content">
                    <h4>Notas</h4>
                    <ul class="collection">
                        <li class="collection-item avatar" v-for="(ln, index) in listadoNotas" :key="index">
                            <i class="material-icons circle">content_paste</i>
                            <span class="title">{{ln.created_at}}</span>
                            <p>{{ln.descripcion}}</p>
                            <a href="#!" class="secondary-content">
                                <i class="material-icons">grade</i>
                            </a>
                        </li>
                    </ul>
                </div>
                <div class="modal-footer">
                    <a href="#!"
                        class="modal-action modal-close waves-effect waves-red btn border-round accent-4 red">Cerrar</a>
                </div>
            </div>
        </div>
      </div>
    </div>
    <!-- invoice action  -->
    <div class="col xl2 m4 s12">
      <div class="card invoice-action-wrapper">
        <div class="card-content">
          <div class="invoice-action-btn">
            <a href="#" class="btn-block btn waves-effect waves-light red" @click="cambiar(false)">
              <span>Regresar</span>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</section><!-- START RIGHT SIDEBAR NAV -->

<!-- END RIGHT SIDEBAR NAV -->
          </div>
          <div class="content-overlay"></div>
        </div>
      </div>
        </div>
            <!-- modal agregar nuevo presupuesto -->
            <div id="modal1" class="modal modal-fixed-footer" style="width: 85% !important ;">
                <form class="formValidate0" id="formValidate0"
                    @submit.prevent="agregarPresupuesto(totalUnitario,total)">
                    <div class="modal-content">
                        <center>
                            <h5><b>Nuevo presupuesto</b></h5>
                        </center>
                        <div id="html-view-validations">
                            <div class="row">
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">date_range</i>
                                    <input id="birthdate" v-model="presupuesto.fecha" class="datepicker"
                                        placeholder="Fecha">
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">message</i>
                                    <textarea id="icon_prefix" v-model="presupuesto.notas"
                                        class="materialize-textarea"></textarea>
                                    <label for="icon_prefix">Observaciones</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">clear_all</i>
                                    <select v-model="presupuesto.eje" class="listadoclase">
                                        <option value="Seleccionar">Seleccionar</option>
                                        <option value="2021">2021</option>
                                        <option value="2022">2022</option>
                                        <option value="2023">2023</option>
                                        <option value="2024">2024</option>
                                        <option value="2025">2025</option>
                                        <option value="2026">2026</option>
                                        <option value="2027">2027</option>
                                        <option value="2028">2028</option>
                                    </select>
                                    <label for="icon_telephone">Ejercicio</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">face</i>
                                    <input id="icon_prefix19" v-model="presupuesto.nombre_para" type="text"
                                        class="validate" readonly>
                                    <label for="icon_prefix1"></label>
                                </div>
                                <div class="col s12">
                                    <a class="btn-floating waves-effect waves-light modal-trigger red right"
                                        href="#modal2"><i class="material-icons">add</i></a>
                                </div>
                                <div class="col s12">
                                    <table class="responsive-table highlight">
                                        <thead>
                                            <tr>
                                                <th>Concepto</th>
                                                <th>Periodo</th>
                                                <th>Cantidad</th>
                                                <th>Precio Unitario</th>
                                                <th>Precio Total</th>
                                                <th></th>


                                            </tr>
                                        </thead>
                                        <tbody>
                                            <tr v-for="(ca, index) in conceptosAgregar" :key="index">
                                                <td>{{ ca.concepto }}</td>
                                                <td>{{ ca.periodoPago }}</td>
                                                <td>{{ ca.cantidad }}</td>
                                                <td>$ {{ formatPrice(ca.precioUnitario) }}</td>
                                                <td>$ {{ formatPrice(ca.precioTotal) }}</td>
                                                <td class="center-align"><a href="#"
                                                        @click="eliminarConceptoAlta(index)"><i
                                                            class="material-icons pink-text">clear</i></a>
                                                </td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer" style="height: 98px !important; ">
                        <div class="row pt-1">
                            <div class="col s12 m6 l6">
                                Total Precio Unitario: $ {{ formatPrice(totalUnitario) }}
                            </div>
                            <div class="col s12 m6 l6">
                                Total: $ {{ formatPrice(total) }}
                            </div>
                        </div>
                        <div class="col s12 m12 l12 pt-1">
                            <a href="#!"
                                class="modal-action modal-close waves-effect waves-red btn border-round accent-4 red">Cancelar</a>
                            <button class="btn waves-effect waves-light border-round accent-4 gradient-45deg-green-teal"
                                type="submit" name="action">Guardar
                                <i class="material-icons right">send</i>
                            </button>
                        </div>
                    </div>

                </form>
            </div>
            <!-- cierra modal agregar nuevo presupuesto -->
            <!-- modal agregar nuevo concepto -->
            <div id="modal2" class="modal modal-fixed-footer">
                <form class="formValidate0" id="formValidate0" method="get">
                    <div class="modal-content">
                        <center>
                            <h5><b>Nuevo concepto</b></h5>
                        </center>
                        <div id="html-view-validations">
                            <div class="row">
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">chrome_reader_mode</i>
                                    <select class="listadoclase" @change="selectSubCuentas($event)" v-model="nuevoConcepto.id_cuenta_contable">
                                        <option value="Seleccionar">Seleccionar</option>
                                        <option v-for="(cuentas, index) in cuentas" :key="index" :value="cuentas.id_cuenta_contable">{{cuentas.nombre}}</option>
                                    </select>
                                    <label for="icon_telephone99">Cuenta contable</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">chrome_reader_mode</i>
                                    <select class="listadoclase" v-model="nuevoConcepto.id_sub_cuenta_contable">
                                        <option value="Seleccionar">Seleccionar</option>
                                       <option v-for="(sb, index) in subCuentas" :key="index" :value="sb.id_sub_cuenta_contable">{{sb.nombre}}</option>
                                    </select>
                                    <label for="icon_telephone89">Sub-cuenta contable</label>
                                </div>
                                <div class="input-field col s12">
                                    <i class="material-icons prefix">border_color</i>
                                    <input id="icon_prefix1" v-model="nuevoConcepto.concepto" type="text"
                                        class="validate">
                                    <label for="icon_prefix1">Concepto</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">loyalty</i>
                                    <input id="icon_telephone" v-model="nuevoConcepto.cantidad" type="number"
                                        class="validate">
                                    <label for="icon_telephone">Cantidad(pza)</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">attach_money</i>
                                    <input id="icon_prefix2" v-model="nuevoConcepto.precioUnitario"
                                        v-on:change="calculaTotal()" type="text" class="validate">
                                    <label for="icon_prefix2">Precio Unitario</label>
                                </div>
                    
                                <div class="input-field col s12 m12 l12">
                                    <i class="material-icons prefix">attach_money</i>
                                    <input v-model="nuevoConcepto.precioTotal" type="text" class="validate"
                                        placeholder="Precio total" readonly>
                                </div>
                                <div class="input-field col s12 m4 l4">
                                    <i class="material-icons prefix">clear_all</i>
                                    <select v-model="nuevoConcepto.periodoPago" class="listadoclase" @change="periodoConcepto()">
                                        <option value="Seleccionar">Seleccionar</option>
                                        <option value="Trimestral">Trimestral</option>
                                        <option value="Semestral">Semestral</option>
                                        <option value="Anual">Anual</option>
                                        <option value="Pago único">Pago único</option>
                                    </select>
                                    <label for="icon_telephone">Periodo pago</label>
                                </div>
                                <div class="input-field col s12 m8 l8">
                                    <i class="material-icons prefix">message</i>
                                    <textarea id="icon_prefix3" v-model="nuevoConcepto.observaciones"
                                        class="materialize-textarea"></textarea>
                                    <label for="icon_prefix3">Observaciones</label>
                                </div>
                                <div v-for="(valor, index) in detalleconceptoinserta" :key="index">
                                    
                                    <div class="input-field col s12 m6 l6">
                                        <i class="material-icons prefix">date_range</i>
                                        <select class="listadoclase" v-model="valor.mes">
                                            <option value="Enero">Enero</option>
                                            <option value="Febrero">Febrero</option>
                                            <option value="Marzo">Marzo</option>
                                            <option value="Abril">Abril</option>
                                            <option value="Mayo">Mayo</option>
                                            <option value="Junio">Junio</option>
                                            <option value="Julio">Julio</option>
                                            <option value="Agosto">Agosto</option>
                                            <option value="Septiembre">Septiembre</option>
                                            <option value="Octubre">Octubre</option>
                                            <option value="Noviembre">Noviembre</option>
                                            <option value="Diciembre">Diciembre</option>
                                        </select>
                                        <label :for="`icon_telephone89${index}`">Mes de pago</label>
                                    </div>
                                    <div class="input-field col s12 m6 l6">
                                        <i class="material-icons prefix">attach_money</i>
                                        <input :id="`icon_prefix29${index}`" type="text" class="validate" v-model="valor.monto" placeholder="">
                                        <label :for="`icon_prefix29${index}`" class="active">Monto</label>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <a href="#!"
                            class="modal-action modal-close waves-effect waves-effect btn red accent-4 border-round" @click.prevent="limpiaModal()">Cancelar</a>
                        <button class="btn waves-effect waves-light gradient-45deg-green-teal border-round"
                            v-on:click="agregarConcepto()" type="button" name="action">Guardar
                            <i class="material-icons right">send</i>
                        </button>
                        <!-- <a href="#!" v-on:click="agregarConcepto()" class="modal-action waves-effect waves-green btn green">Guardar<i class="material-icons right">send</i></a> -->
                    </div>
                </form>
            </div>
            <!-- cierra modal agregar nuevo concepto -->
            <!-- modal editar presupuesto -->
            <div id="modaleditarpresupuesto" class="modal modal-fixed-footer" style="width: 85% !important ;">
                <form class="formValidate0" id="formValidate0"
                    @submit.prevent="editarSolicitudConceptoModal(totalUnitarioEditar,totalEditar)">
                    <div class="modal-content">
                        <center>
                            <h5><b>Editar solicitud {{ folioEditar }}</b></h5>
                        </center>
                        <div id="html-view-validations">
                            <div class="row">
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">date_range</i>
                                    <input id="birthdate" v-model="presupuestoEditarModal.fecha" class="datepicker"
                                        placeholder="Fecha" readonly>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">message</i>
                                    <textarea id="icon_prefix" v-model="presupuestoEditarModal.notas"
                                        class="materialize-textarea" placeholder=""></textarea>
                                    <label for="icon_prefix">Observaciones</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">clear_all</i>
                                    <select v-model="presupuestoEditarModal.eje" class="listadoclase">
                                        <option value="Seleccionar">Seleccionar</option>
                                        <option value="2021">2021</option>
                                        <option value="2022">2022</option>
                                        <option value="2023">2023</option>
                                        <option value="2024">2024</option>
                                        <option value="2025">2025</option>
                                        <option value="2026">2026</option>
                                        <option value="2027">2027</option>
                                        <option value="2028">2028</option>
                                    </select>
                                    <label for="icon_telephone">Ejercicio</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">face</i>
                                    <select id="icon_telephone2" v-model="presupuestoEditarModal.para" class="listadoclase" disabled>
                                        <option v-for="(usuario, index) in usuarios" :key="index" :value="usuario.id">{{usuario.name}}</option>
                                    </select>
                                    <label for="icon_telephone2">Para</label>
                                </div>
                                <div class="col s12">
                                    <a class="btn-floating waves-effect waves-light modal-trigger red right"
                                        href="#modaleditaragregarnuevoconcepto"><i class="material-icons">add</i></a>
                                </div>
                                <div class="col s12">
                                    
                                    <table class="responsive-table highlight">
                                        <thead>
                                            <tr>
                                                <th>Concepto</th>
                                                <th>Periodo</th>
                                                <th>Cantidad</th>
                                                <th>Precio Unitario</th>
                                                <th>Precio Total</th>
                                                <th></th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <tr v-for="(lc, index) in conceptoListado" :key="index">
                                                <td>{{ lc.concepto }}</td>
                                                <td>{{ lc.periodo }}</td>
                                                <td>{{ lc.cantidad }}</td>
                                                <td>$ {{ formatPrice(lc.precioUnitario) }}</td>
                                                <td>$ {{ formatPrice(lc.precioTotal) }}</td>
                                                <td class="center-align">
                                                    <a href="#modalEditarConcepto" @click="infoEditarConcepto(lc),selectSubCuentas2(lc.id_cuenta_contable)"
                                                        class="modal-trigger"><i
                                                            class="material-icons blue-text">edit</i></a>
                                                    <a href="#" @click.prevent="eliminarConcepto(lc)"><i
                                                            class="material-icons red-text">clear</i></a>
                                                </td>
                                            </tr>
                                        </tbody>
                                    </table>
                                    <br><br>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer" style="height: 98px !important; ">
                        <div class="row pt-1">
                            <div class="col s12 m6 l6">
                                Total Precio Unitario: $ {{ formatPrice(totalUnitarioEditar) }}
                            </div>
                            <div class="col s12 m6 l6">
                                Total: $ {{ formatPrice(totalEditar) }}
                            </div>
                        </div>
                        <div class="col s12 m12 l12 pt-1">
                            <a href="#!"
                                class="modal-action modal-close waves-effect waves-red btn border-round accent-4 red">Cancelar</a>
                            <button class="btn waves-effect waves-light border-round accent-4 gradient-45deg-green-teal"
                                type="submit" name="action">Guardar
                                <i class="material-icons right">send</i>
                            </button>
                        </div>
                    </div>

                </form>
            </div>
            <!-- cierra modal editar presupuesto -->
            <!-- modal agregar nuevo concepto editar -->
            <div id="modaleditaragregarnuevoconcepto" class="modal modal-fixed-footer">
                <form class="formValidate0" id="formValidate0" method="get">
                    <div class="modal-content">
                        <center>
                            <h5><b>Nuevo concepto</b></h5>
                        </center>
                        <div id="html-view-validations">
                            <div class="row">
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">chrome_reader_mode</i>
                                    <select class="listadoclase" @change="selectSubCuentas($event)" v-model="nuevoConceptoModalEditar.id_cuenta_contable">
                                        <option value="Seleccionar">Seleccionar</option>
                                        <option v-for="(cuentas, index) in cuentas" :key="index" :value="cuentas.id_cuenta_contable">{{cuentas.nombre}}</option>
                                    </select>
                                    <label for="icon_telephone198">Cuenta contable</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">chrome_reader_mode</i>
                                    <select class="listadoclase" v-model="nuevoConceptoModalEditar.id_sub_cuenta_contable">
                                        <option value="Seleccionar">Seleccionar</option>
                                       <option v-for="(sb, index) in subCuentas" :key="index" :value="sb.id_sub_cuenta_contable">{{sb.nombre}}</option>
                                    </select>
                                    <label for="icon_telephone196">Sub-cuenta contable</label>
                                </div>
                                <div class="input-field col s12">
                                    <i class="material-icons prefix">border_color</i>
                                    <input id="icon_prefix11" v-model="nuevoConceptoModalEditar.concepto" type="text"
                                        class="validate">
                                    <label for="icon_prefix11">Concepto</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">loyalty</i>
                                    <input id="icon_telephone28" v-model="nuevoConceptoModalEditar.cantidad"
                                        type="number" class="validate">
                                    <label for="icon_telephone28">Cantidad(pza)</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">attach_money</i>
                                    <input id="icon_prefix13" v-model="nuevoConceptoModalEditar.precioUnitario"
                                        v-on:change="calculaTotalAltaEditar()" type="text" class="validate">
                                    <label for="icon_prefix13">Precio Unitario</label>
                                </div>
                                <div class="input-field col s12 m12 l12">
                                    <i class="material-icons prefix">attach_money</i>
                                    <input v-model="nuevoConceptoModalEditar.precioTotal" type="text" class="validate"
                                        placeholder="Precio total" readonly>
                                </div>
                                <div class="input-field col s12 m4 l4">
                                    <i class="material-icons prefix">clear_all</i>
                                    <select v-model="nuevoConceptoModalEditar.periodoPago" class="listadoclase" @change="periodoConceptoEditarNuevo()">
                                        <option value="Seleccionar">Seleccionar</option>
                                        <option value="Trimestral">Trimestral</option>
                                        <option value="Semestral">Semestral</option>
                                        <option value="Anual">Anual</option>
                                        <option value="Pago único">Pago único</option>
                                    </select>
                                    <label for="icon_telephone">Periodo pago</label>
                                </div>
                                <div class="input-field col s12 m8 l8">
                                    <i class="material-icons prefix">message</i>
                                    <textarea id="icon_prefix17" v-model="nuevoConceptoModalEditar.observaciones"
                                        class="materialize-textarea"></textarea>
                                    <label for="icon_prefix17">Observaciones</label>
                                </div>
                                <div v-for="(valor, index) in detalleconceptoinserta" :key="index">
                                    
                                    <div class="input-field col s12 m6 l6">
                                        <i class="material-icons prefix">date_range</i>
                                        <select class="listadoclase" v-model="valor.mes">
                                            <option value="Enero">Enero</option>
                                            <option value="Febrero">Febrero</option>
                                            <option value="Marzo">Marzo</option>
                                            <option value="Abril">Abril</option>
                                            <option value="Mayo">Mayo</option>
                                            <option value="Junio">Junio</option>
                                            <option value="Julio">Julio</option>
                                            <option value="Agosto">Agosto</option>
                                            <option value="Septiembre">Septiembre</option>
                                            <option value="Octubre">Octubre</option>
                                            <option value="Noviembre">Noviembre</option>
                                            <option value="Diciembre">Diciembre</option>
                                        </select>
                                        <label :for="`icon_telephone895${index}`">Mes de pago</label>
                                    </div>
                                    <div class="input-field col s12 m6 l6">
                                        <i class="material-icons prefix">attach_money</i>
                                        <input :id="`icon_prefix295${index}`" type="text" class="validate" v-model="valor.monto" placeholder="">
                                        <label :for="`icon_prefix295${index}`" class="active">Monto</label>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <a href="#!"
                            class="modal-action modal-close waves-effect waves-effect btn red accent-4 border-round" @click.prevent="limpiaModal()">Cancelar</a>
                        <button class="btn waves-effect waves-light gradient-45deg-green-teal border-round"
                            @click="agregarConceptoModalEditar()" type="button" name="action">Guardar
                            <i class="material-icons right">send</i>
                        </button>
                        <!-- <a href="#!" v-on:click="agregarConcepto()" class="modal-action waves-effect waves-green btn green">Guardar<i class="material-icons right">send</i></a> -->
                    </div>
                </form>
            </div>
            <!-- cierra modal agregar nuevo concepto editar-->

            <!-- modal editar concepto -->
            <div id="modalEditarConcepto" class="modal modal-fixed-footer">
                <form class="formValidate0" id="formValidate0" method="get">
                    <div class="modal-content">
                        <center>
                            <h5><b>Editar concepto</b></h5>
                        </center>
                        <div id="html-view-validations">
                            <div class="row">
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">chrome_reader_mode</i>
                                    <select class="listadoclase" @change="selectSubCuentas($event)" v-model="editarConcepto.id_cuenta_contable">
                                        <option value="Seleccionar">Seleccionar</option>
                                        <option v-for="(cuentas, index) in cuentas" :key="index" :value="cuentas.id_cuenta_contable">{{cuentas.nombre}}</option>
                                    </select>
                                    <label for="icon_telephone95">Cuenta contable</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">chrome_reader_mode</i>
                                    <select v-model="editarConcepto.id_sub_cuenta_contable" class="listadoclase">
                                        <option value="Seleccionar">Seleccionar</option>
                                       <option v-for="(sb, idx) in subCuentas" :key="idx" :value="sb.id_sub_cuenta_contable">{{sb.nombre}}</option>
                                    </select>
                                    <label for="icon_telephone94">Sub-cuenta contable</label>
                                </div>
                                <div class="input-field col s12">
                                    <i class="material-icons prefix">border_color</i>
                                    <input id="icon_prefix111" v-model="editarConcepto.concepto" type="text"
                                        class="validate" placeholder="">
                                    <label for="icon_prefix111">Concepto</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">loyalty</i>
                                    <input id="icon_telephone22" v-model="editarConcepto.cantidad"
                                        v-on:change="calculaTotalEditar()" type="number" class="validate" placeholder="">
                                    <label for="icon_telephone22">Cantidad(pza)</label>
                                </div>
                                <div class="input-field col s12 m6 l6">
                                    <i class="material-icons prefix">attach_money</i>
                                    <input id="icon_prefix133" v-model="editarConcepto.precioUnitario"
                                        v-on:change="calculaTotalEditar()" type="text" class="validate" placeholder="">
                                    <label for="icon_prefix133">Precio Unitario</label>
                                </div>
                                <div class="input-field col s12 m12 l12">
                                    <i class="material-icons prefix">attach_money</i>
                                    <input v-model="editarConcepto.precioTotal" type="text" class="validate"
                                        placeholder="Precio total" readonly>
                                </div>
                                <div class="input-field col s12 m4 l4">
                                    <i class="material-icons prefix">clear_all</i>
                                    <select v-model="editarConcepto.periodoPago" class="listadoclase" disabled>
                                        <option value="Seleccionar">Seleccionar</option>
                                        <option value="Trimestral">Trimestral</option>
                                        <option value="Semestral">Semestral</option>
                                        <option value="Anual">Anual</option>
                                        <option value="Pago único">Pago único</option>
                                    </select>
                                    <label for="icon_telephone">Periodo pago</label>
                                </div>
                                <div class="input-field col s12 m8 l8">
                                    <i class="material-icons prefix">message</i>
                                    <textarea id="icon_prefix174" v-model="editarConcepto.observaciones"
                                        class="materialize-textarea" placeholder=""></textarea>
                                    <label for="icon_prefix174">Observaciones</label>
                                </div>
                                  <div v-for="(valor, index) in editarConcepto.detalleConcepto" :key="index">
                                    
                                    <div class="input-field col s12 m6 l6">
                                        <i class="material-icons prefix">date_range</i>
                                        <select class="listadoclase" v-model="valor.mes">
                                            <option value="Enero">Enero</option>
                                            <option value="Febrero">Febrero</option>
                                            <option value="Marzo">Marzo</option>
                                            <option value="Abril">Abril</option>
                                            <option value="Mayo">Mayo</option>
                                            <option value="Junio">Junio</option>
                                            <option value="Julio">Julio</option>
                                            <option value="Agosto">Agosto</option>
                                            <option value="Septiembre">Septiembre</option>
                                            <option value="Octubre">Octubre</option>
                                            <option value="Noviembre">Noviembre</option>
                                            <option value="Diciembre">Diciembre</option>
                                        </select>
                                        <label :for="`icon_telephone8956${index}`">Mes de pago</label>
                                    </div>
                                    <div class="input-field col s12 m6 l6">
                                        <i class="material-icons prefix">attach_money</i>
                                        <input :id="`icon_prefix65${index}`" type="number" class="validate" v-model="valor.monto">
                                        <label :for="`icon_prefix65${index}`" class="active">Monto</label>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <a href="#!"
                            class="modal-action modal-close waves-effect waves-effect btn red accent-4 border-round">Cancelar</a>
                        <button class="btn waves-effect waves-light gradient-45deg-green-teal border-round"
                            @click.prevent="editarConceptoPresupuesto()" type="button" name="action">Guardar
                            <i class="material-icons right">send</i>
                        </button>
                        <!-- <a href="#!" v-on:click="agregarConcepto()" class="modal-action waves-effect waves-green btn green">Guardar<i class="material-icons right">send</i></a> -->
                    </div>
                </form>
            </div>
            <!-- cierra modal editar concepto-->
    </div>
</template>

<script>
import notificacionesComponent from './NotificacionEstatusComponent.vue'
import VueTimeago from 'vue-timeago'

Vue.use(VueTimeago, {
  name: 'Timeago', // Component name, `Timeago` by default
  locale: 'en', // Default locale
  // We use `date-fns` under the hood
  // So you can use all locales from it
  locales: {
    'zh-CN': require('date-fns/locale/zh_cn'),
    ja: require('date-fns/locale/ja')
  }
})
    export default {
         mounted() {
            this.getPresupuesto(); 
        },
        components: {
            notificacionesComponent,
        },
        props: [
            'id_usuario_logeado',
            'name_usuario_logeado',
            'urlprueba',
            'usuario_contabilidad',
            ],
        data() {
            return {
                vistainfo: false,
                presupuestos: [],
                tiempoBusqueda: '',
                searchpresupuesto: '',
                pagination: {
                    'total': 0,
                    'current_page': 0,
                    'per_page': 0,
                    'last_page': 0,
                    'from': 0,
                    'to': 0
                },
                offset: 2,
                hoy: '',
                usuarios: [],
                id_presupuesto: '',
                solicitante: '',
                rol: '',
                presupuesto: {
                    folio: '',
                    area: '',
                    solicitante: '',
                    fecha: '',
                    eje: 'Seleccionar',
                    para: '',
                    nombre_para: '',
                    notas: '',
                    iva_final: 0,
                    subtotal_final: '',
                    total_final: ''
                },
                nuevoConcepto: {
                    folio: '',
                    concepto: '',
                    cantidad: '',
                    precioUnitario: '',
                    iva: 0,
                    precioTotal: '',
                    periodoPago: 'Seleccionar',
                    observaciones: '',
                    id_cuenta_contable: 'Seleccionar',
                    id_sub_cuenta_contable: 'Seleccionar',
                    detalleConcepto: ''
                },
                nuevaNotificacion: {
                    descripcion: '',
                    id_usuario_de: '',
                    id_usuario_para: '',
                    modulo: 'Presupuesto Anual',
                    url:'presupuesto-anual',
                    detalle_notificacion: ''
                },
                conceptosAgregar: [],
                conceptoListado: [],
                presupuestoEditarModal: {
                    id_pres: '',
                    fecha: '',
                    eje: 'Seleccionar',
                    para: 'Seleccionar Persona',
                    notas: '',
                    iva_final: 0,
                    subtotal_final: '',
                    total_final: ''
                },
                nuevoConceptoModalEditar: {
                    folio: '',
                    concepto: '',
                    cantidad: '',
                    precioUnitario: '',
                    iva: 0,
                    precioTotal: '',
                    periodoPago: 'Seleccionar',
                    observaciones: '',
                    id_cuenta_contable: 'Seleccionar',
                    id_sub_cuenta_contable: 'Seleccionar',
                    detalleConcepto: ''
                },
                editarConcepto: {
                    id_elem: '',
                    folio: '',
                    concepto: '',
                    cantidad: '',
                    precioUnitario: '',
                    iva: 0,
                    precioTotal: '',
                    periodoPago: 'Seleccionar',
                    observaciones: '',
                    id_cuenta_contable: 'Seleccionar',
                    id_sub_cuenta_contable: 'Seleccionar',
                    detalleConcepto: ''
                },
                folioEditar: '',
                infoPresupuestoEditar: [],
                usuarioPara: [],
                usuarioSolicitante: [],
                informacionDetallePresupuesto: [],
                estatuspresupuesto: '',
                textoestatuspresupuesto: '',
                notas: {
                    id_pres: '',
                    descripcion: ''
                },
                listadoNotas: [],
                para_quien: '',
                pres_usuario_solicitante: '',
                nombre_solicitante : '',
                bandera_componente:'1',
                cuentas:[],
                subCuentas:[],
                tablaRegistroCuenta:[],
                nuevoPDF:{ tablaRegistroCuenta: '',
                            conceptoListado:'',
                            totalPresupuesto:''
                        },
                ciclaInputs: '',
                detalleconceptoinserta:[]
            }
        },
        ready: function () {
            $('select').material_select();
        },
        computed: {
            isActived: function () {
                return this.pagination.current_page;
            },
            pageNumber: function () {
                if (!this.pagination.to) {
                    return [];

                }
                var from = this.pagination.current_page - this.offset; // bandera offset
                if (from < 1) {
                    from = 1
                }
                var to = from + (this.offset * 2);
                if (to >= this.pagination.last_page) {
                    to = this.pagination.last_page;
                }
                var pageArray = [];
                while (from <= to) {
                    pageArray.push(from);
                    from++;
                }

                return pageArray;
            },
            total() {
                return this.conceptosAgregar.reduce((total, ca) => {
                    return total + Number(ca.precioTotal);
                }, 0);
            },

            totalUnitario() {
                return this.conceptosAgregar.reduce((total, ca) => {
                    return total + Number(ca.precioUnitario);
                }, 0);
            },
            totalEditar() {
                return this.conceptoListado.reduce((total, lc) => {
                    return total + Number(lc.precioTotal);
                }, 0);
            },

            totalUnitarioEditar() {
                return this.conceptoListado.reduce((total, lc) => {
                    return total + Number(lc.precioUnitario);
                }, 0);
            },
            totalMontoConcepto() {
                return this.detalleconceptoinserta.reduce((total, valor) => {
                    return total + Number(valor.monto);
                }, 0);
            },
            totalMontoConceptoEditar() {
                return this.editarConcepto.detalleConcepto.reduce((total, valor) => {
                    return total + Number(valor.monto);
                }, 0);
            }
        },
        methods: {
            getPresupuesto(page) {
                var url = `presupuesto-anual/lista?buscador=${this.searchpresupuesto}&page=` + page;
                axios.get(url).then(response => {        
                    this.presupuestos = response.data.presupuesto.data
                    this.pagination = response.data.pagination
                    this.hoy = response.data.hoy
                    this.usuarios = response.data.usuarios
                    this.presupuesto.para = response.data.usuario_edson.id
                    this.presupuesto.nombre_para = response.data.usuario_edson.name
                    this.cuentas = response.data.cuentas
                    this.solicitante = response.data.solicitante
                    this.rol = response.data.rol
                    this.id_presupuesto = response.data.ultimo_id_presupuesto
                })
            },
            buscarPresupuesto() {
                // $("#searchspinerpresupuesto").html(
                //     '<div class="spinner-border" role="status" style="float:right;"><span class="sr-only">Cargando...</span></div>'
                // )
                clearTimeout(this.tiempoBusqueda)
                this.tiempoBusqueda = setTimeout(this.getPresupuesto, 600)
            },
            changePage: function (page) {
                this.pagination.current_page = page;
                this.getPresupuesto(page);
            },
            infoPresupuesto() {
                if (this.id_presupuesto == '' || this.id_presupuesto == null) {
                    var id_presupuesto = 1;
                } else {
                    var id_presupuesto = parseFloat(this.id_presupuesto) + parseFloat(1);
                }
                this.presupuesto.folio = `1000${id_presupuesto}`;
                this.presupuesto.fecha = this.hoy;
                this.presupuesto.area = this.rol;
                this.presupuesto.solicitante = this.solicitante;
            },
            agregarPresupuesto(totalUnitario, total) {
                this.presupuesto.subtotal_final = totalUnitario;
                this.presupuesto.total_final = total;
                if (this.presupuesto.notas === '') {
                    swal('Debes ingresar una observación',{
                        position: 'center',
                        icon: 'error', 
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.presupuesto.eje === 'Seleccionar') {
                    swal('Debes seleccionar un ejercicio',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.presupuesto.para === 'Seleccionar Persona') {
                    swal('Debes seleccionar una persona',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                const nuevoPresupuesto = {
                    'presupuesto': this.presupuesto,
                    'conceptosAgregar': this.conceptosAgregar
                };
                if (this.conceptosAgregar == '') {
                    swal('Debes agregar un concepto al presupuesto',{
                        position: 'center',
                        icon: 'error', 
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                axios.post('presupuesto/create', nuevoPresupuesto).then((res) => {
                    this.presupuesto = {
                        folio: '',
                        area: '',
                        solicitante: '',
                        fecha: '',
                        eje: 'Seleccionar',
                        para: '',
                        nombre_para: '',
                        notas: '',
                        iva_final: 0,
                        subtotal_final: '',
                        total_final: ''
                    }
                    this.search = ''
                    swal('Se guardo correctamente!',{
                        position: 'center',
                        icon: 'success',
                        buttons: false,
                        timer: 1500
                    });
                    this.getPresupuesto();
                    this.conceptosAgregar = []
                })
                $('#modal1').modal('close');
            },
            calculaTotal() {
                var total = parseFloat(this.nuevoConcepto.cantidad) * parseFloat(this.nuevoConcepto.precioUnitario)
                this.nuevoConcepto.precioTotal = parseFloat(total)
            },
            agregarConcepto() {
                if (this.nuevoConcepto.id_cuenta_contable === 'Seleccionar') {
                    swal('Debes seleccionar una cuenta contable',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.nuevoConcepto.id_sub_cuenta_contable === 'Seleccionar') {
                    swal('Debes seleccionar una sub-cuenta contable',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.nuevoConcepto.concepto === '') {
                    swal('Debes agregar el concepto',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }

                if (this.nuevoConcepto.cantidad === '') {
                    swal('Debes agregar una cantidad',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.nuevoConcepto.precioUnitario === '') {
                    swal('Debes agregar un precio',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.nuevoConcepto.periodoPago === 'Seleccionar') {
                    swal('Debes seleccionar un periodo de pago',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.nuevoConcepto.observaciones === '') {
                    swal('Debes agregar una observacion',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if(this.totalMontoConcepto.toFixed(2) > this.nuevoConcepto.precioTotal){
                    swal('La suma de los montos es mayor que el total.',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 2500
                    })
                    return;                  
                }
                else if(this.totalMontoConcepto.toFixed(2) < this.nuevoConcepto.precioTotal){
                    swal('La suma de los montos es menor que el total.',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 2500
                    })
                    return;                  
                }
                this.nuevoConcepto.detalleConcepto = this.detalleconceptoinserta;
                this.conceptosAgregar.push(this.nuevoConcepto)
                this.nuevoConcepto = {
                    folio: '',
                    concepto: '',
                    cantidad: '',
                    precioUnitario: '',
                    iva: 0,
                    precioTotal: '',
                    periodoPago: 'Seleccionar',
                    observaciones: '',
                    id_cuenta_contable: 'Seleccionar',
                    id_sub_cuenta_contable: 'Seleccionar',
                    detalleConcepto: ''
                };
                this.detalleconceptoinserta= []
                $('#modal2').modal('close');
            },
            eliminarConceptoAlta(x) {
                this.conceptosAgregar.splice(x, 1);
            },
            formatPrice(value) {
                let val = (value / 1).toFixed(3).replace(',', '.')
                return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
            },
            listadoConceptos(presupuesto) {
                this.infoPresupuestoEditar = presupuesto;
                this.presupuestoEditarModal.id_pres = presupuesto.id_pres;
                this.presupuestoEditarModal.fecha = presupuesto.fecha;
                this.presupuestoEditarModal.eje = presupuesto.eje;
                this.presupuestoEditarModal.para = presupuesto.para;
                this.presupuestoEditarModal.notas = presupuesto.notas;
                this.folioEditar = presupuesto.folio;
                var folio = presupuesto.folio;
                axios.get(`conceptos/listado/${folio}`).then((res) => {
                    this.conceptoListado = res.data.listadoConceptos

                })
            },
            editarSolicitudConceptoModal(totalUnitarioEditar,totalEditar) {
                this.presupuestoEditarModal.subtotal_final = totalUnitarioEditar;
                this.presupuestoEditarModal.total_final = totalEditar;
                const editarPresupuesto = this.presupuestoEditarModal;
                axios.put('presupuesto/editar', editarPresupuesto).then((res) => {
                    swal('Se edito correctamente',{
                        position: 'center',
                        icon: 'info',
                        buttons: false,
                        timer: 1500
                    })
                    $('#modaleditarpresupuesto').modal('close');
                    this.getPresupuesto();

                })
            },
            calculaTotalAltaEditar() {
                var total = parseFloat(this.nuevoConceptoModalEditar.cantidad) * parseFloat(this.nuevoConceptoModalEditar
                    .precioUnitario)
                this.nuevoConceptoModalEditar.precioTotal = total
            },
            eliminarConcepto(lc) {
                swal({
                    title: 'Estas seguro de eliminar?',
                    text: "¡No podrás revertir esto!",
                    icon: 'warning',
                    dangerMode: true,
                    buttons: {
                    cancel: 'No!',
                    delete: 'Si!'
                    }
                }).then(function (willDelete) {
                    if (willDelete) { 
                        var id_elem = lc.id_elem;
                        axios.delete(`conceptos/eliminar/${id_elem}`).then((res) => {
                            swal('Se elimino correctamente',{
                                position: 'center',
                                icon: 'error',
                                buttons: false,
                                timer: 1500
                            })
                        })
                    }
                })
                var folio = lc.folio;
                axios.get(`conceptos/listado/${folio}`).then(response => {
                    this.conceptoListado = response.data.listadoConceptos

                })
            },
            agregarConceptoModalEditar() {
                if (this.nuevoConceptoModalEditar.id_cuenta_contable === 'Seleccionar') {
                    swal('Debes seleccionar una cuenta contable',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.nuevoConceptoModalEditar.id_sub_cuenta_contable === 'Seleccionar') {
                    swal('Debes seleccionar una sub-cuenta contable',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.nuevoConceptoModalEditar.concepto === '') {
                    swal('Debes agregar el concepto',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }

                if (this.nuevoConceptoModalEditar.cantidad === '') {
                    swal('Debes agregar una cantidad',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.nuevoConceptoModalEditar.precioUnitario === '') {
                    swal('Debes agregar un precio',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.nuevoConceptoModalEditar.periodoPago === 'Seleccionar') {
                    swal('Debes seleccionar un periodo de pago',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if (this.nuevoConceptoModalEditar.observaciones === '') {
                    swal('Debes agregar una observacion',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                if(this.totalMontoConcepto > this.nuevoConceptoModalEditar.precioTotal){
                    swal('La suma de los montos es mayor que el total.',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 2500
                    })
                    return;                  
                }
                else if(this.totalMontoConcepto < this.nuevoConceptoModalEditar.precioTotal){
                    swal('La suma de los montos es menor que el total.',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 2500
                    })
                    return;                  
                }
                this.nuevoConceptoModalEditar.detalleConcepto = this.detalleconceptoinserta;
                var folio = this.folioEditar;
                const altaPresupuestoEditar = this.nuevoConceptoModalEditar;
                axios.post(`conceptos/alta/editar/${folio}`, altaPresupuestoEditar).then((res) => {
                    swal('Se guardo correctamente!',{
                        position: 'center',
                        icon: 'success',
                        buttons: false,
                        timer: 1500
                    });
                    this.nuevoConceptoModalEditar = {
                        folio: '',
                        concepto: '',
                        cantidad: '',
                        precioUnitario: '',
                        iva: 0,
                        precioTotal: '',
                        periodoPago: 'Seleccionar',
                        observaciones: '',
                        id_cuenta_contable: 'Seleccionar',
                        id_sub_cuenta_contable: 'Seleccionar',
                        detalleConcepto: ''
                    }
                    var presupuesto = this.infoPresupuestoEditar;
                    this.detalleconceptoinserta= [];
                    this.listadoConceptos(presupuesto);
                    $('#modaleditaragregarnuevoconcepto').modal('close');
                })

            },
            infoEditarConcepto(lc) {
                this.editarConcepto.id_elem = lc.id_elem;
                this.editarConcepto.folio = lc.folio;
                this.editarConcepto.concepto = lc.concepto;
                this.editarConcepto.cantidad = lc.cantidad;
                this.editarConcepto.precioUnitario = lc.precioUnitario;
                this.editarConcepto.precioTotal = lc.precioTotal;
                this.editarConcepto.periodoPago = lc.periodo;
                this.editarConcepto.observaciones = lc.evenObser;
                this.editarConcepto.id_cuenta_contable = lc.id_cuenta_contable;
                this.editarConcepto.id_sub_cuenta_contable = lc.id_sub_cuenta_contable;
                this.editarConcepto.detalleConcepto = lc.detalleConceptosEditar;
            },
            editarConceptoPresupuesto() {
                const editarConcepto = this.editarConcepto;
                
                if(this.totalMontoConceptoEditar.toFixed(2) > this.editarConcepto.precioTotal){
                    swal('La suma de los montos es mayor que el total.',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 2500
                    })
                    return;                  
                }
                else if(this.totalMontoConceptoEditar.toFixed(2) < this.editarConcepto.precioTotal){
                    swal('La suma de los montos es menor que el total.',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 2500
                    })
                    return;                  
                }
                axios.put('conceptos/editar', editarConcepto).then((res) => {
                    swal('Se edito correctamente',{
                        position: 'center',
                        icon: 'info',
                        buttons: false,
                        timer: 1500
                    })
                    $('#modalEditarConcepto').modal('close');
                    var folio = this.editarConcepto.folio;
                    axios.get(`conceptos/listado/${folio}`).then((res) => {
                        this.conceptoListado = res.data.listadoConceptos
                    })
                })

            },
            calculaTotalEditar() {
                var total = parseFloat(this.editarConcepto.cantidad) * parseFloat(this.editarConcepto.precioUnitario)
                this.editarConcepto.precioTotal = total
            },
            infoPresupuestoDetalle(presupuesto) {
                this.informacionDetallePresupuesto.id_pres = presupuesto.id_pres;
                this.informacionDetallePresupuesto.eje = presupuesto.eje;
                 this.informacionDetallePresupuesto.fecha = presupuesto.fecha;
                this.informacionDetallePresupuesto.area = presupuesto.area;
                this.informacionDetallePresupuesto.solicitante = presupuesto.solicitante;
                this.informacionDetallePresupuesto.folio = presupuesto.folio;
                this.informacionDetallePresupuesto.estatus = presupuesto.estatus;
                this.informacionDetallePresupuesto.para = presupuesto.para;
                this.estatuspresupuesto = presupuesto.estatus;
                this.textoestatuspresupuesto = presupuesto.estatus_nuevo;
                this.informacionDetallePresupuesto.estatus_nuevo = presupuesto.estatus_nuevo;
                this.informacionDetallePresupuesto.estatus_class = presupuesto.estatus_class;
                this.informacionDetallePresupuesto.notas = presupuesto.notas;
                this.informacionDetallePresupuesto.subtotal_final = presupuesto.subtotal_final;
                this.informacionDetallePresupuesto.total_final = presupuesto.total_final;
                axios.get(`presupuesto/detalle/${presupuesto.para}/${presupuesto.solicitante}`).then(res => {
                    this.usuarioSolicitante = res.data.usuarioSolicitante[0]
                    this.pres_usuario_solicitante = res.data.usuarioSolicitante[0].id
                    this.usuarioPara = res.data.usuarioPara[0];
                    this.para_quien = res.data.usuarioPara[0].id
                    this.nombre_solicitante = presupuesto.solicitante


                })
            },
            listaNotas(id_pres) {
                var id_pres = id_pres;
                axios.get(`notas/listado/${id_pres}`).then((res) => {
                    this.listadoNotas = res.data
                })

            },
            editarStatus(id_pres) {
                var id_pres = id_pres;
                var bandera = 1;
                axios.put(`presupuesto/editarestatus/${this.estatuspresupuesto}/${id_pres}/${bandera}`).then((res) => {
                    swal('El estatus de edito correctamente!',{
                        position: 'center',
                        icon: 'success',
                        buttons: false,
                        timer: 1500
                    });        
                    this.getPresupuesto();
                    this.vistainfo = false;
                })
            },
            editarStatusPorAprobar(id_pres) {
                var bandera = 2;
                this.nuevaNotificacion.descripcion = `El estatus del presupuesto ${id_pres} ha sido actualizado.`;
                this.nuevaNotificacion.id_usuario_de = this.para_quien;
                this.nuevaNotificacion.id_usuario_para = this.pres_usuario_solicitante;
                if(this.estatuspresupuesto == 2 ){
                    this.nuevaNotificacion.detalle_notificacion = 'Aprobada';
                }
                if(this.estatuspresupuesto == 3 ){
                    this.nuevaNotificacion.detalle_notificacion = 'Rechazada';
                }

                var id_pres = id_pres;
                axios.put(`presupuesto/editarestatus/${this.estatuspresupuesto}/${id_pres}/${bandera}`).then((res) => {
                    swal('El estatus de edito correctamente!',{
                        position: 'center',
                        icon: 'success',
                        buttons: false,
                        timer: 1500
                    });
                    const nuevaNotificacion = this.nuevaNotificacion;
                    axios.post('notificaciones-solicitante',nuevaNotificacion).then(response =>{
                        this.nuevaNotificacion= {
                            descripcion: '',
                            id_usuario_de: '',
                            id_usuario_para: '',
                            modulo:'Presupuesto Anual',
                            url:'presupuesto-anual',
                            detalle_notificacion: ''
                        };    
                    })
                })
                this.getPresupuesto();
                this.vistainfo = false;
            },
             editarStatusAprobadaRechazada(id_pres) {
                var bandera = 4;
                this.nuevaNotificacion.descripcion = `El estatus del presupuesto ${id_pres} ha sido actualizado.`;
                this.nuevaNotificacion.id_usuario_de = this.id_usuario_logeado;
               if(this.estatuspresupuesto == 4 ){
                    this.nuevaNotificacion.detalle_notificacion = 'Por aprobar';
                    this.nuevaNotificacion.id_usuario_para = this.para_quien;
                }
                if(this.estatuspresupuesto == 3 ){
                    this.nuevaNotificacion.detalle_notificacion = 'Rechazada';
                    this.nuevaNotificacion.id_usuario_para = this.pres_usuario_solicitante;
                }

                var id_pres = id_pres;
                axios.put(`presupuesto/editarestatus/${this.estatuspresupuesto}/${id_pres}/${bandera}`).then((res) => {
                    swal('El estatus de edito correctamente!',{
                        position: 'center',
                        icon: 'success',
                        buttons: false,
                        timer: 1500
                    });
                    const nuevaNotificacion = this.nuevaNotificacion;
                    axios.post('notificaciones-solicitante',nuevaNotificacion).then(response =>{
                        this.nuevaNotificacion= {
                            descripcion: '',
                            id_usuario_de: '',
                            id_usuario_para: '',
                            modulo:'Presupuesto Anual',
                            url:'presupuesto-anual',
                            detalle_notificacion: ''
                        };    
                    })                
                })
                this.getPresupuesto();
                this.vistainfo = false;
            },
            estatusRevision(id_pres) {
                console.log(this.usuario_contabilidad)
                var bandera = 3;
                this.nuevaNotificacion.descripcion = `Tienes una nueva solicitud de ${this.nombre_solicitante}`;
                this.nuevaNotificacion.id_usuario_de = this.pres_usuario_solicitante;
                this.nuevaNotificacion.id_usuario_para = this.usuario_contabilidad;
                if(this.estatuspresupuesto == 1 ){
                    this.nuevaNotificacion.detalle_notificacion = 'En revisión';
                }
                var id_pres = id_pres;
                axios.put(`presupuesto/editarestatus/${this.estatuspresupuesto}/${id_pres}/${bandera}`).then(response => {
                    swal('El estatus de edito correctamente!',{
                        position: 'center',
                        icon: 'success', 
                        buttons: false,
                        timer: 1500
                    });
                    const nuevaNotificacion = this.nuevaNotificacion;
                    axios.post('notificaciones',nuevaNotificacion).then(response =>{
                        this.nuevaNotificacion= {
                            descripcion: '',
                            id_usuario_de: '',
                            id_usuario_para: '',
                            modulo:'Presupuesto Anual',
                            url:'presupuesto-anual',
                            detalle_notificacion: ''
                        };
                    })
                })
                this.getPresupuesto();
                this.vistainfo = false;

            },
            agregarNota(presupuesto) {
                if (this.notas.descripcion === '') {
                    swal('Debes agregar una descripción',{
                        position: 'center',
                        icon: 'error',
                        buttons: false,
                        timer: 1500
                    })
                    return;
                }
                const notas = this.notas;
                axios.post('notas/create', notas).then((res) => {
                    swal('Se creo correctamente!',{
                        position: 'center',
                        icon: 'success',
                        buttons: false,
                        timer: 1500
                    });
                    this.notas = {
                        id_pres: '',
                        descripcion: ''
                    }
                    $('#modalnotas').modal('close');
                })
            },
            asignarIdPresupuesto(id_pres) {
                this.notas.id_pres = id_pres;
            },
            selectSubCuentas(event){
            axios.get(`sub-cuentas-contables/select/${event.target.value}`).then(response=>{
              console.log("esto es respuesta:"+response)
              this.subCuentas = response.data
            })
            },
            selectSubCuentas2(id){
                axios.get(`sub-cuentas-contables/select/${id}`).then(response=>{
                this.subCuentas = response.data
            })
            },
            nombreCuentaContable(folio){
                $('#modalloading').modal('open');
                axios.get(`presupuesto-anual/tablaCuenta/${folio}`).then(response => {
                    this.tablaRegistroCuenta = response.data
                    this.nuevoPDF.tablaRegistroCuenta = this.tablaRegistroCuenta;
                this.nuevoPDF.conceptoListado =  this.conceptoListado;
                this.nuevoPDF.infoPresupuestoEditar = this.infoPresupuestoEditar;
                axios({
                        url: 'presupuesto-anual/tabla-amortizacion',
                        method: 'POST',
                        data: this.nuevoPDF,
                        responseType: 'blob', // important
                    }).then((response) => {
                        const url = window.URL.createObjectURL(new Blob([response.data]));
                        const link = document.createElement('a');
                        link.href = url;
                        link.setAttribute('download', 'file.pdf');
                        document.body.appendChild(link);
                        link.click();
                        $('#modalloading').modal('close');
                    });
                    this.nuevoPDF={ tablaRegistroCuenta: '',
                            conceptoListado:'',
                            totalPresupuesto:''
                        }
                })
            },
            periodoConcepto(){
                this.ciclaInputs = '';
                this.detalleconceptoinserta= [];
                if(this.nuevoConcepto.periodoPago == 'Anual'){
                    var montoDividido = Number(this.nuevoConcepto.precioTotal) / 12;
                    this.ciclaInputs = 12;
                    for (var i = 0; i < this.ciclaInputs; i++) {
                        this.detalleconceptoinserta.push({mes:'',monto: montoDividido})
                    }
                }
                else if(this.nuevoConcepto.periodoPago == 'Semestral'){
                    var montoDividido = Number(this.nuevoConcepto.precioTotal) / 2;
                    this.ciclaInputs = 2;
                    for (var i = 0; i < this.ciclaInputs; i++) {
                        this.detalleconceptoinserta.push({mes:'',monto:montoDividido})
                    }
                }
                else if(this.nuevoConcepto.periodoPago == 'Trimestral'){
                    var montoDividido = Number(this.nuevoConcepto.precioTotal) / 4;
                    this.ciclaInputs = 4;
                     for (var i = 0; i < this.ciclaInputs; i++) {
                        this.detalleconceptoinserta.push({mes:'',monto:montoDividido})
                    }
                }
                else if(this.nuevoConcepto.periodoPago == 'Pago único'){
                    var montoDividido = Number(this.nuevoConcepto.precioTotal);
                    this.ciclaInputs = 1;
                    for (var i = 0; i < this.ciclaInputs; i++) {
                        this.detalleconceptoinserta.push({mes:'',monto:montoDividido})
                    }
                }
               

            },
            periodoConceptoEditarNuevo(){
                this.ciclaInputs = '';
                this.detalleconceptoinserta= [];
                if(this.nuevoConceptoModalEditar.periodoPago == 'Anual'){
                     var montoDivididoEditar = Number(this.nuevoConceptoModalEditar.precioTotal) / 12;
                    this.ciclaInputs = 12;
                    for (var i = 0; i < this.ciclaInputs; i++) {
                        this.detalleconceptoinserta.push({mes:'',monto:montoDivididoEditar})
                    }
                }
                else if(this.nuevoConceptoModalEditar.periodoPago == 'Semestral'){
                    var montoDivididoEditar = Number(this.nuevoConceptoModalEditar.precioTotal) / 2;
                    this.ciclaInputs = 2;
                    for (var i = 0; i < this.ciclaInputs; i++) {
                        this.detalleconceptoinserta.push({mes:'',monto:montoDivididoEditar})
                    }
                }
                else if(this.nuevoConceptoModalEditar.periodoPago == 'Trimestral'){
                    var montoDivididoEditar = Number(this.nuevoConceptoModalEditar.precioTotal) / 4;
                    this.ciclaInputs = 4;
                     for (var i = 0; i < this.ciclaInputs; i++) {
                        this.detalleconceptoinserta.push({mes:'',monto:montoDivididoEditar})
                    }
                }
                else if(this.nuevoConceptoModalEditar.periodoPago == 'Pago único'){
                    var montoDivididoEditar = Number(this.nuevoConceptoModalEditar.precioTotal);
                    this.ciclaInputs = 1;
                    for (var i = 0; i < this.ciclaInputs; i++) {
                        this.detalleconceptoinserta.push({mes:'',monto:montoDivididoEditar})
                    }
                }
            },
            limpiaModal(){
                this.nuevoConcepto= {
                    folio: '',
                    concepto: '',
                    cantidad: '',
                    precioUnitario: '',
                    iva: 0,
                    precioTotal: '',
                    periodoPago: 'Seleccionar',
                    observaciones: '',
                    id_cuenta_contable: 'Seleccionar',
                    id_sub_cuenta_contable: 'Seleccionar',
                    detalleConcepto: ''
                }
                // conceptosAgregar: [],
                // conceptoListado: [],
                
                this.nuevoConceptoModalEditar= {
                    folio: '',
                    concepto: '',
                    cantidad: '',
                    precioUnitario: '',
                    iva: 0,
                    precioTotal: '',
                    periodoPago: 'Seleccionar',
                    observaciones: '',
                    id_cuenta_contable: 'Seleccionar',
                    id_sub_cuenta_contable: 'Seleccionar',
                    detalleConcepto: ''
                }
            },
            cambiar(valor){
                this.vistainfo = valor;
            }
           
        },
        updated() {
             $('.listadoclase').formSelect();
            //  this.$refs.notificacionesComponente.getNotificaciones();
        }
 
    }
$(document).ready(function(){
  $("#modaleditarpresupuesto").modal({dismissible: false})
})

</script>
