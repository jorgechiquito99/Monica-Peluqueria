(function( $ ) {
	"use strict";

	/*
     * [ Switch wc currency ] - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
     */
	var initSwitchCurrency = function() {
		$( 'body' ).on( 'click', '.currency-item', function() {
			var currency = $( this ).data( 'currency' );
			$.cookie( 'jms_currency', currency, { path: '/' });
			location.reload();
		});
	};

	/**
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 * Enable masonry grid for gallery
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 */
	var galleryMasonry = function() {
		if ( ($('.adiva-gallery').hasClass('gallery-design-masonry')) || ($('.adiva-gallery').hasClass('gallery-design-grid')) ) {
			// initialize Masonry after all images have loaded
			var $container = $('.gallery-wrapper');

			$container.imagesLoaded(function() {
				$container.isotope({
				  	itemSelector: '.gallery-wrapper .item',
				  	columnWidth: '.gallery-wrapper .item',
				  	percentPosition: true,
					layoutMode: 'masonry'
				})
			});
		}
	}

	/**
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 * Enable masonry grid for blog
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 */
	var blogMasonry = function() {
		if ( ! $('.adiva-loop-blog').hasClass('masonry-container') || $('.adiva-loop-blog').hasClass('blog-type-default') ) return;

		var $container = $('.masonry-container');

		// initialize Masonry after all images have loaded
		$container.imagesLoaded(function() {
			$container.isotope({
			  	itemSelector: '.blog-post-loop',
			  	columnWidth: '.blog-post-loop',
			  	percentPosition: true
			})
		});
	}

	// Init openswatch update images
	function initOpenSwatch() {
		$( '.imageswatch-list-variations a' ).on( 'click', function(e) {
			e.preventDefault();

			var src = $( this ).data( 'thumb' );
			if ( src != '' ) {
				$( this ).closest( '.product-item' ).find( 'img.wp-post-image' ).first().attr( 'src', src );
				$( this ).closest( '.product-item' ).find( 'img.wp-post-image' ).first().attr( 'srcset', src );
			}
		});
	}

	/**
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 * Enable masonry grid for portfolio
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 */
	var portfolioMasonry = function() {
		if ( ! $('.portfolio-layout').hasClass('portfolio-masonry') ) return;

		var el = $('.portfolio-masonry');
		// initialize Masonry after all images have loaded
		el.imagesLoaded( function() {
			el.isotope({
			  	itemSelector: '.portfolio',
			  	columnWidth: '.portfolio',
			  	percentPosition: true
			});

			$('.portfolio-filter').on('click', 'a', function(e) {
				e.preventDefault();
				$('.portfolio-filter').find('.selected').removeClass('selected');
				$(this).addClass('selected');

				var filterValue = $(this).attr('data-filter');

				el.isotope({
					filter: filterValue
				});
			});
		});
	}

	/**
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 * Enable masonry grid for product
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 */
	var productMasonry = function() {
		var el = $('.wc-product-masonry');

		// initialize Masonry after all images have loaded
		el.each( function( i, val ) {
			var _option = $( this ).data( 'masonry' );

			if ( _option !== undefined ) {
				var _selector = _option.selector,
					_width    = _option.columnWidth,
					_layout   = _option.layoutMode;

				$( this ).imagesLoaded( function() {
					$( val ).isotope( {
						layoutMode : _layout,
						itemSelector: _selector,
						percentPosition: true,
						masonry: {
							columnWidth: _width
						},
						transitionDuration: '0.5s',
					} );
				});

				$('.masonry-filter').on('click', 'a', function(e) {
					e.preventDefault();
					$('.masonry-filter').find('.active').removeClass('active');
					$(this).addClass('active');
					var filterValue = $(this).attr('data-filter');
					el.isotope({
						filter: filterValue
					});
				});
			}
		});
	}

	/**
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 * Load more button for blog || product and portfolio
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 */

	function ajaxLoadMoreItem() {
		var btn_loadmore = $('.adiva-ajax-loadmore');

		btn_loadmore.each( function( i, val ) {
			var data_option = $(this).data( 'load-more' );

			if ( data_option !== undefined ) {
				var page      = data_option.page,
					container = data_option.container,
					layout    = data_option.layout,
					isLoading = false,
					anchor    = $( val ).find( 'a' ),
					next      = $( anchor ).attr( 'href' ),
					i 		  = 2;

				// Load more
				if ( layout == 'loadmore' ) {
					$( val ).on( 'click', 'a', function( e ) {
						e.preventDefault();
						anchor = $( val ).find( 'a' );
						next   = $( anchor ).attr( 'href' );

						$( anchor ).html( '<i class="fa fa-circle-o-notch fa-spin"></i>' + adiva_settings.loading );

						getData();
					});
				}

				// Infinite Scroll Loading
				if ( layout == 'infinite' ) {
					var animationFrame = function() {
						anchor = $( val ).find( 'a' );
						next   = $( anchor ).attr( 'href' );

						var bottomOffset = $( '.' + container ).offset().top + $( '.' + container ).height() - $( window ).scrollTop();

						if ( bottomOffset < window.innerHeight && bottomOffset > 0 && ! isLoading ) {
							if ( ! next )
								return;
							isLoading = true;
							$( anchor ).html( '<i class="fa fa-circle-o-notch fa-spin"></i>' + adiva_settings.loading );

							getData();
						}
					};

					var scrollHandler = function() {
						requestAnimationFrame( animationFrame );
					};

					$( window ).scroll( scrollHandler );
				}


				var getData = function() {
					$.get( next + '', function( data ) {
						var content    = $( '.' + container, data ).wrapInner( '' ).html(),
							newElement = $( '.' + container, data ).find( '.blog-post-loop, .product, .portfolio' );

						$( content ).imagesLoaded( function() {
							next = $( anchor, data ).attr( 'href' );
							$( '.' + container ).append( newElement ).isotope( 'appended', newElement );
						});

						$( anchor ).text( adiva_settings.load_more );

						if ( page > i ) {
							if ( adiva_settings !== undefined && adiva_settings.permalink == 'plain' ) {
								var link = next.replace( /paged=+[0-9]+/gi, 'paged=' + ( i + 1 ) );
							} else {
								var link = next.replace( /page\/+[0-9]+\//gi, 'page/' + ( i + 1 ) + '/' );
							}

							$( anchor ).attr( 'href', link );
						} else {
							$( anchor ).text( adiva_settings.no_more_item );
							$( anchor ).removeAttr( 'href' ).addClass( 'disabled' );
						}
						isLoading = false;
						i++;
					});
				}
			}

		});

	}


	/**
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 * Banner hover effect with jquery panr
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 */
	function bannerHoverParallax() {
		$('.banner-box.banner-hover-parallax').panr({
			sensitivity: 35,
			scale: false,
			scaleOnHover: true,
			scaleTo: 1.15,
			scaleDuration: .34,
			panY: true,
			panX: true,
			panDuration: 0.3,
			resetPanOnMouseLeave: true
		});
	}

	/*
     * [ Page Loader ] - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
     */
    var initPreLoader = function() {
	    $(window).load(function(){
	        if($('.preloader').length > 0){
	            $('.preloader').delay(300).fadeOut('slow');
	        }
	    });

    }

	// Initialize WC quantity adjust.
	function QuantityAdjust() {
		if ( ! String.prototype.getDecimals ) {
            String.prototype.getDecimals = function() {
                var num = this,
                    match = ('' + num).match(/(?:\.(\d+))?(?:[eE]([+-]?\d+))?$/);
                if ( ! match ) {
                    return 0;
                }
                return Math.max( 0, ( match[1] ? match[1].length : 0 ) - ( match[2] ? +match[2] : 0 ) );
            }
        }

        $( document ).on( 'click', '.plus, .minus', function() {
            // Get values
            var $qty        = $( this ).closest( '.quantity' ).find( '.qty'),
                currentVal  = parseFloat( $qty.val() ),
                max         = parseFloat( $qty.attr( 'max' ) ),
                min         = parseFloat( $qty.attr( 'min' ) ),
                step        = $qty.attr( 'step' );

            // Format values
            if ( ! currentVal || currentVal === '' || currentVal === 'NaN' ) currentVal = 0;
            if ( max === '' || max === 'NaN' ) max = '';
            if ( min === '' || min === 'NaN' ) min = 0;
            if ( step === 'any' || step === '' || step === undefined || parseFloat( step ) === 'NaN' ) step = '1';

            // Change the value
            if ( $( this ).is( '.plus' ) ) {
                if ( max && ( currentVal >= max ) ) {
                    $qty.val( max );
                } else {
                    $qty.val( ( currentVal + parseFloat( step )).toFixed( step.getDecimals() ) );
                }
            } else {
                if ( min && ( currentVal <= min ) ) {
                    $qty.val( min );
                } else if ( currentVal > 0 ) {
                    $qty.val( ( currentVal - parseFloat( step )).toFixed( step.getDecimals() ) );
                }
            }

            // Trigger change event
            $qty.trigger( 'change' );
        });
	}

	function BackToTop() {
        $(window).scroll(function(){
            if ($(this).scrollTop() >= 500) {
                $('#backtop').fadeIn();
            } else {
                $('#backtop').fadeOut();
            }
	    });

		$( '#backtop' ).on( 'click', function(e) {
		    $('html, body').animate({
                scrollTop : 0
            }, 800);
		    return false;
	    });
    }


	function initToggleMenuLeft() {
		$( '.header-3 .header-left .menu-toggle' ).on( 'click', function (e) {
			e.preventDefault();
			$( 'body' ).toggleClass( 'has-menu-toggle-left' );
		});

		$( '#sidebar-open-overlay' ).on( 'click', function() {
			$( 'body' ).removeClass( 'has-menu-toggle-left' );
		});
	}

	function initToggleMenuSidebar() {
		$( '.header-action .menu-toggle' ).on( 'click', function (e) {
			e.preventDefault();
			$( 'body' ).toggleClass( 'has-toggle-sidebar' );
			$( '#sidebar-open-overlay' ).on( 'click', function() {
				$( 'body' ).removeClass( 'has-toggle-sidebar' );
			});
		});
	}

	function overlapHeader() {
		var topBar        = $( 'header.header-wrapper .topbar' );
		var	topBarHeight  = topBar.outerHeight();

		if( ! $('.header-wrapper').hasClass('header-overlap') || topBar.length == 0 ) return;

		$('.header-overlap .main-header').css('top',  topBarHeight);
	}

	function initStickyHeader() {
		var header        = $( 'header.header-wrapper' );
		var	headerHeight  = header.outerHeight();

		if( ! $('body').hasClass('has-sticky-header') || header.length == 0 ) return;

		$(window).scroll(function(){
			var scroll = $(window).scrollTop();

			if ( $('body').hasClass('has-sticky-header') && ( scroll >= headerHeight + 200 ) ) {
				$('.header-sticky').addClass('show');
			} else {
				$('.header-sticky').removeClass('show');
			}
		});

	}


	/*
	 * [ Product Quickview ] - - - - - - - - - - - - - - - - - - - - - - - -
	 */
	function ProductQuickView() {
		$( 'body' ).on( 'click', '.btn-quickview', function( e ) {
			var _this = $( this );

			_this.addClass('loading');

			var id = _this.attr( 'data-product' ),
				data = {
					action: 'adiva_quickview',
					product: id
				};

			$.post( adiva_settings.ajaxurl, data, function( response ) {
				if ( typeof $.fn.magnificPopup != 'undefined' ) {
					$.magnificPopup.open( {
						items: {
							src: response
						},
						removalDelay: 500,
						callbacks: {
							beforeOpen: function() {
								this.st.image.markup = this.st.image.markup.replace('mfp-figure', 'mfp-figure mfp-with-anim');
								this.st.mainClass = 'mfp-left-horizontal';
							}
						},
					} );
				}

				setTimeout(function() {
					if ( $( '.product-quickview form' ).hasClass( 'variations_form' ) ) {
						$( '.product-quickview form.variations_form' ).wc_variation_form();
						$( '.product-quickview select' ).trigger( 'change' );
					}
				}, 100);

				_this.removeClass('loading');

				SlickSlider();

				$( '.images' ).imagesLoaded( function() {
					var imgHeight = $( '.product-quickview .images' ).outerHeight();

					if ( $( window ).width() > 767 ) {
						$( '.product-quickview .wc-single-product > div' ).css({
							'height': imgHeight
						});
					}

				});
			} );

			e.preventDefault();
			e.stopPropagation();
		} );
	}

	// slick slider
	var SlickSlider = function() {
		$( '.thumbnail-slider' ).not( '.slick-initialized' ).slick();
	};

	//Sticky sidebar
	function StickySidebar() {
		var smart_sidebar = $('.smart-sidebar');
		if ( $( 'body.admin-bar.has-sticky-header' ).length > 0 ) {
            smart_sidebar.theiaStickySidebar({additionalMarginTop: 150});
        } else if ( $( 'body.has-sticky-header' ).length > 0 ) {
            smart_sidebar.theiaStickySidebar({additionalMarginTop: 120});
        } else if ( $( 'body.admin-bar' ).length > 0 ) {
            smart_sidebar.theiaStickySidebar({additionalMarginTop: 80});
        } else {
            smart_sidebar.theiaStickySidebar({additionalMarginTop: 50});
        }

		var sticky_column = $('.adiva-sticky-column');
        sticky_column.theiaStickySidebar({additionalMarginTop: 150});
	}

	// Carousel Slider
	var initCarousel = function() {
		var el = $( '.owl-carousel' );

		el.each( function( i, val ) {
			var _option = $( this ).data( 'carousel' );

			if ( _option !== undefined ) {
				var _selector   	   = _option.selector,
					_itemDesktop   	   = _option.itemDesktop,
					_itemSmallDesktop  = _option.itemSmallDesktop,
					_itemTablet        = _option.itemTablet,
					_itemMobile        = _option.itemMobile,
					_itemSmallMobile   = _option.itemSmallMobile,
					_margin            = _option.margin,
					_navigation        = _option.navigation,
					_pagination        = _option.pagination,
					_autoplay          = _option.autoplay,
					_loop        	   = _option.loop;

					if ( _margin == '0' ) {
						var marginSmallMobile = 0;
						var marginMobile = 0;
						var marginTablet = 0;
						var marginSmallDesktop = 0;
					}

					if ( _margin == '10' ) {
						var marginSmallMobile = 10;
						var marginMobile = 10;
						var marginTablet = 10;
						var marginSmallDesktop = 10;
					}

					if ( _margin == '20' ) {
						var marginSmallMobile = 10;
						var marginMobile = 10;
						var marginTablet = 20;
						var marginSmallDesktop = 20;
					}

					if ( _margin == '30' || _margin == '40' || _margin == '50' || _margin == '60' ) {
						var marginSmallMobile = 10;
						var marginMobile = 10;
						var marginTablet = 20;
						var marginSmallDesktop = 30;
					}

				var rtl = false;
			    if ($('body').hasClass('rtl')) rtl = true;

				$(_selector).owlCarousel({
			        responsive : {
			        	320 : {
			        		items: _itemSmallMobile,
							margin: marginSmallMobile
			        	},
						445 : {
			        		items: _itemMobile,
							margin: marginMobile
			        	},
						768 : {
			        		items: _itemTablet,
							margin: marginTablet
			        	},
					    991 : {
					        items: _itemSmallDesktop,
							margin: marginSmallDesktop
					    },
					    1199 : {
					        items: _itemDesktop,
					    }
					},
					rtl: rtl,
			        margin: _margin,
			        dots: _pagination,
			        nav: _navigation,
			        autoplay: _autoplay,
			        loop: _loop,
			        smartSpeed: 1000,
					navText: ['<i class="icon-arrow prev"></i>','<i class="icon-arrow next"></i>']
			    });

			}
		} );

	}

	/*
	 * [ Parse URL to array ] - - - - - - - - - - - - - - - - - - - - - - - - -
	 */
	function Parse_Url_To_Array( url ) {
		if ( url.search( '&' ) == -1 )
			return false;

		var data = {}, queries, temp, i;

		// Split into key/value pairs
		queries = url.split( "&" );

		// Convert the array of strings into an object
		for ( i = 0; i < queries.length; i++ ) {
			temp = queries[ i ].split( '=' );
			data[ temp[ 0 ] ] = temp[ 1 ];
		}
		return data;
	}

	function AddToWishList() {
		$( 'body' ).on( 'click', '.yith-wcwl-add-button .add_to_wishlist', function( e ) {
			e.preventDefault();
		});

        $( document ).ajaxComplete( function( event, xhr, settings ) {
            var url = settings.url;
            var data_request = ( typeof settings.data != 'undefined' ) ? settings.data : '';

            if ( settings.data !== undefined ) {
                var data_array_url = Parse_Url_To_Array( settings.data );

                if ( data_array_url.action == 'add_to_wishlist' ) {

                    $( 'body > .notice-cart-wrapper' ).remove();
                    var content_notice = '<div class="notice-cart-wrapper"><div class="notice-cart"><div class="icon-notice"><i class="fa fa-heart-o"></i></div><div class="text-notice"><div> ' + xhr.responseJSON.message + ' </div><a class="db" href="' + xhr.responseJSON.wishlist_url + '">' + adiva_settings.viewall_wishlist + '</a></div></div></div>';
                    $( 'body' ).append( content_notice );

					var close = $( '<span class="close-notice sl icon-close"></span>' );

					$('body').on('click', '.close-notice', function() {
						$( this ).closest( '.notice-cart-wrapper' ).removeClass( 'active' );
					});

					$( 'body .notice-cart' ).prepend( close );

					setTimeout( function() {
						$( 'body > .notice-cart-wrapper' ).addClass( 'active' );
					}, '0' );

					setTimeout( function() {
						$( 'body > .notice-cart-wrapper' ).removeClass( 'active' );
					}, '5000' );

                }

            }
        });

		$( 'body' ).on( 'click', '.yith-wcwl-remove-button a', function( e ) {
			e.preventDefault();

			var _this   = $(this);
			var parent  = _this.closest( '.yith-wcwl-add-to-wishlist' );
			var loading = parent.find( '.yith-wcwl-remove-button .ajax-loading' );
			var add     = parent.find( '.yith-wcwl-add-button .add_to_wishlist' );

			_this.css( 'opacity', '0' );
			loading.css( 'visibility', 'visible' );
			add.css( 'opacity', '1' );

			var data_request = {
				action: 'adiva_remove_product_wishlist',
				_nonce: _nonce_adiva,
				product_id: _this.attr( 'data-product-id' )
			};

			$.ajax( {
				type: 'POST',
				url: adiva_settings.ajaxurl,
				data: data_request,
				success: function( val ) {
					if( val.status == 'true' ) {
						// Remove
						loading.css( 'visibility', 'hidden' );

						// Hide remove
						parent.find( '.yith-wcwl-remove-button' ).removeClass('show').hide();

						// Show add
						parent.find( '.yith-wcwl-add-button' ).removeClass('hide').show();

						// Show remove
						_this.css( 'opacity', '1' );
					}
				}
			} );

		} );
	}

	function ShowMobileMenu() {
		$( 'body' ).on( 'click', '.menu-toggle .menu-button', function (e) {
			e.preventDefault();

			if ( ! $( 'body' ).hasClass( 'menu-mobile-open-menu' ) ) {
				$( 'body' ).addClass( 'menu-mobile-open-menu' );
			}

			$('#sidebar-open-overlay, .adiva-mobile-menu .close-menu' ).on( 'click', function() {
				$( 'body' ).removeClass( 'menu-mobile-open-menu' );
			});
		});
	}

	// Accordion mobile menu
	function DropdownMenuMobile() {
        if( $( window ).width() < 992 ) {
            $( 'ul.mobile-menu li.menu-item-has-children' ).append( '<span class="holder"></span>' );
    		$( 'body' ).on('click','.holder',function() {
    			var el = $( this ).closest( 'li' );
    			if ( el.hasClass( 'open' ) ) {
    				el.removeClass( 'open' );
    				el.find( 'li' ).removeClass( 'open' );
    				el.find( 'ul' ).slideUp();
    			} else {
    				el.addClass( 'open' );
    				el.children( 'ul' ).slideDown();
    				el.siblings( 'li' ).children( 'ul' ).slideUp();
    				el.siblings( 'li' ).removeClass( 'open' );
    				el.siblings( 'li' ).find( 'li' ).removeClass( 'open' );
    				el.siblings( 'li' ).find( 'ul' ).slideUp();
    			}
    		});
        }
	};

	function ShopFilterToggle() {
		$('.shop-filter-toggle').on('click', function (e) {
			var $this = $(this);
			$this.toggleClass('open');

			$('.shop-filter').slideToggle( '100' );
		});
	}

	// Init Countdown
	function initCountdown() {
		/**
		 *-------------------------------------------------------------------------------------------------------------------------------------------
		 * Sale final date countdown
		 *-------------------------------------------------------------------------------------------------------------------------------------------
		 */
		$( '.adiva-countdown' ).each(function(i, val){
			var time = moment.tz( $(this).data('end-date'), $(this).data('timezone') );
			$( this ).countdown( time.toDate(), function( event ) {
				$( this ).html( event.strftime(''
					+ '<span class="countdown-days">%-D <span>' + adiva_settings.days + '</span></span> '
					+ '<span class="countdown-hours">%H <span>' + adiva_settings.hours + '</span></span> '
					+ '<span class="countdown-min">%M <span>' + adiva_settings.mins + '</span></span> '
					+ '<span class="countdown-sec">%S <span>' + adiva_settings.secs + '</span></span>'));
			});
		});
	}

	// Init elevate zoom
	var initZoom = function() {
		if ( $( '.image-zoom' ).length > 0 ) {
			var img = $( '.image-zoom' ), img_src = $( '.image-zoom' ).data( 'src' );

			img.zoom({
				touch: false,
				url: img_src,
				magnify: 1.7
			});
		}
	}

	// Init Magnific Popup
	var initMagnificPopup = function() {
		if ( $( '.wc-single-video' ).length > 0 ) {
			$('.wc-popup-url').magnificPopup({
				type:'iframe',
			});
		}

		$('[data-rel="mfp[gallery]"]').magnificPopup({
			type: 'image',
			removalDelay: 500,
			callbacks: {
				beforeOpen: function() {
					this.st.image.markup = this.st.image.markup.replace('mfp-figure', 'mfp-figure mfp-with-anim');
					this.st.mainClass = 'mfp-left-horizontal';
				}
			},
			image: {
				verticalFit: true
			},
			gallery: {
				enabled: true,
				navigateByImgClick: false
			},
		});

		$('.p-thumb').each(function() {
		    $(this).magnificPopup({
		        delegate: '.p-item a',
		        type: 'image',
				removalDelay: 500,
				callbacks: {
					beforeOpen: function() {
						this.st.image.markup = this.st.image.markup.replace('mfp-figure', 'mfp-figure mfp-with-anim');
						this.st.mainClass = 'mfp-left-horizontal';
					}
				},
		        gallery: {
		          	enabled: true
		        },
		    });
		});
	}

	// Extra content on single product ( Help & Shipping - Return )
	var wcShippingContent = function() {
		$( 'body' ).on( 'click', '.adiva-wc-help', function(e) {
			$( '.site' ).after( '<div class="loader"><div class="loader-inner"></div></div>' );

			var data = { action: 'adiva_shipping_return' }

			$.post( adiva_settings.ajaxurl, data, function( response ) {
				$.magnificPopup.open({
					items: {
						src: response
					},
				});
				$( '.loader' ).remove();
			});
			e.preventDefault();
			e.stopPropagation();
		});
	}

	// Refesh mini cart on ajax event
	function refreshMiniCart() {
		$.ajax({
			type: 'POST',
			url: adiva_settings.ajaxurl,
			dataType: 'json',
			data: {
				action:'load_mini_cart'
			},
			success:function( response ) {
				var cart_list = $( '.widget_shopping_cart_content' );
				if ( response.cart_html.length > 0 ) {
					cart_list.html( response.cart_html );
				}
				$( '.cart-contents .cart-count' ).text( response.cart_total );
			}
		});
	}

	// Add to cart alert (in product box)
	var addToCartAlert = function() {
		if ( !$('body').hasClass('add-to-cart-style-alert') ) return;

		$( document ).ajaxComplete( function( event, xhr, settings ) {
            var url = settings.url;
            var data_request = ( typeof settings.data != 'undefined' ) ? settings.data : '';

			if ( url.search( 'wc-ajax=add_to_cart' ) != -1 ) {

				if ( !( settings.data != undefined && xhr.responseJSON != undefined && xhr.responseJSON.cart_hash != undefined ) )
					return false;

				var data_array_url = Parse_Url_To_Array( settings.data );

				$.ajax( {
					type: 'POST',
					url: adiva_settings.ajaxurl,
					data: {
						action: 'add_to_cart_message',
						product_id: data_array_url.product_id
					},
					success: function( val ) {
						showCartAlert(val);
					}
				} );
			}
        });
	}

	function showCartAlert(val) {
		if ( val.message == undefined )
			return false;

		$( 'body > .notice-cart-wrapper' ).remove();
		var content_notice = '<div class="notice-cart-wrapper"><div class="notice-cart"><div class="icon-notice"><i class="fa fa-shopping-bag"></i></div><div class="text-notice">' + val.message + '</div></div></div>';
		$( 'body' ).append( content_notice );

		var close = $( '<span class="close-notice sl icon-close"></span>' );

		$('body').on('click', '.close-notice', function() {
			$( this ).closest( '.notice-cart-wrapper' ).removeClass( 'active' );
		});

		$( 'body .notice-cart' ).prepend( close );

		setTimeout( function() {
			$( 'body > .notice-cart-wrapper' ).addClass( 'active' );
		}, '0' );

		setTimeout( function() {
			$( 'body > .notice-cart-wrapper' ).removeClass( 'active' );
		}, '5000' );
	}

	// Remove item in shopping cart
	function removeCartItem() {
		$('body').on('click', '.mini_cart_item a.remove', function (e) {
			e.preventDefault();
			var $this = $(this);
			var $productKey = $(this).data('item-key');

			$this.closest('li').addClass('loading');

			$this.closest('li').prepend('<div class="wc-loading"></div>').fadeIn( 'fast' );

			// Ajax action
			$.ajax({
				url: adiva_settings.ajaxurl,
				dataType: 'json',
				type: 'POST',
				cache: false,
				headers: { 'cache-control': 'no-cache' },
				data: {
					'action': 'cart_remove_item',
					'item_key': $productKey
				},
				success: function (response) {
					refreshMiniCart();
				}
			});
		});
	}

	/**
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 * Compare button
	 *-------------------------------------------------------------------------------------------------------------------------------------------
	 */
	function compare() {
		var body = $("body"),
			button = $("a.compare");

		body.on("click", "a.compare", function() {
			$(this).addClass("loading");
		});

		body.on("yith_woocompare_open_popup", function() {
			button.removeClass("loading");
			body.addClass("compare-opened");
		});

		body.on('click', '#cboxClose, #cboxOverlay', function() {
			body.removeClass("compare-opened");
		});

	}

	function woocommerceNotices() {
		var notices = '.woocommerce-error, .woocommerce-info, .woocommerce-message, div.wpcf7-response-output, #yith-wcwl-popup-message, .mc4wp-alert, .dokan-store-contact .alert-success, .yith_ywraq_add_item_product_message';

        $('body').on('click', notices, function() {
            var $msg = $(this);
            hideMessage( $msg );
        });

        var showAllMessages = function() {
            $notices.addClass('shown-notice');
        };

        var hideAllMessages = function() {
            hideMessage( $notices );
        };

        var hideMessage = function( $msg ) {
            $msg.removeClass('shown-notice').addClass('hidden-notice');
        };
	}

	function initAddToCart() {
		$('body').on('submit', 'form.cart', function(e) {
            if( adiva_settings.ajax_add_to_cart == false ) return;

            e.preventDefault();

            var $form = $(this),
                $button = $form.find('.button'),
                data = $form.serialize();

            data += '&action=adiva_ajax_add_to_cart';

            if( $button.val() ) {
                data += '&add-to-cart=' + $button.val();
            }

            $button.removeClass( 'added' );
            $button.addClass( 'loading' );

            // Trigger event
            $( document.body ).trigger( 'adding_to_cart', [ $button, data ] );

            $.ajax({
                url: adiva_settings.ajaxurl,
                data: data,
                method: 'POST',
                success: function(response) {
                    if ( ! response ) {
                        return;
                    }

                    // // Redirect to cart option
                    if ( wc_add_to_cart_params.cart_redirect_after_add === 'yes' ) {
                        window.location = wc_add_to_cart_params.cart_url;
                        return;
                    } else {
                        $button.removeClass( 'loading' );
                        $button.addClass( 'loaded' );

                        addCartAction();

                        var fragments = response.fragments;
                        var cart_hash = response.cart_hash;

                        // Replace value
                        if ( fragments ) {
                            $.each( fragments, function( key, value ) {
                                $( key ).replaceWith( value );
                            });
                        }

						// Show notices
                        if( response.notices.indexOf( 'error' ) > 0 ) {
                            $('body').append(response.notices);
                            $button.addClass( 'not-added' );
							$( '.add-to-cart-style-sidebar' ).removeClass( 'has-toggle-cart-sidebar' );
                        } else {
                            // Changes button classes
                            $button.addClass( 'added' );
                            // Trigger event so themes can refresh other areas
                            $( document.body ).trigger( 'added_to_cart', [ fragments, cart_hash, $thisbutton ] );
                        }

                    }

                    $.magnificPopup.close();
                },
                error: function() {
                    console.log('ajax adding to cart error');
                },
                complete: function() {},
            });

        });

		$('body').bind('added_to_cart', function(event, fragments, cart_hash) {
            addCartAction();
        });

		function toggleCartSidebar() {
			$( '.add-to-cart-style-sidebar' ).addClass( 'has-toggle-cart-sidebar' );

			$( '#sidebar-open-overlay, .close-cart' ).on( 'click', function() {
				$( 'body' ).removeClass( 'has-toggle-cart-sidebar' );
			});
		}

		$( 'body' ).on( 'click', '.toggle-sidebar > .cart-contents', function (e) {
			e.preventDefault();
			toggleCartSidebar();
		});


        function addCartAction() {
            if ( $( 'body' ).hasClass('add-to-cart-style-sidebar') ) {
                toggleCartSidebar();
            }

            if ( $( 'body' ).hasClass('add-to-cart-style-alert') ) {
                $( '#header-cart.btn-group' ).addClass( 'cart-opened' );

                setTimeout(function() {
                    $( '#header-cart.btn-group' ).removeClass( 'cart-opened' );
                }, 3000);
            }
        }
	}

    $( document ).ready( function() {
		initPreLoader();
		initSwitchCurrency();
		overlapHeader();
		initStickyHeader();
		initToggleMenuLeft();
		initToggleMenuSidebar();
        BackToTop();
		bannerHoverParallax();
		portfolioMasonry();
		galleryMasonry();
		blogMasonry();
		productMasonry();
		ajaxLoadMoreItem();
		StickySidebar();
		initCarousel();
		ShowMobileMenu();
		DropdownMenuMobile();
		initCountdown();

		//WC
		QuantityAdjust();
		ProductQuickView();
		initOpenSwatch();
		SlickSlider();
		AddToWishList();
		ShopFilterToggle();
		initZoom();
		initMagnificPopup();
		wcShippingContent();
		compare();

		//Add to cart
		initAddToCart();
		removeCartItem();
		woocommerceNotices();

		$("body").on("click", ".add_to_wishlist", function() {
			$(this).parent().addClass("loading");
		});

		//*******************************************************************
		//*Change number of products to be viewed on shop page
		//*******************************************************************/
		$( '.show-products-number' ).change( function() {
			$( this ).closest( 'form' ).submit();
		} );

		if ($('.product-box').hasClass('no-ajax')){
	       	$('.add_to_cart_button').removeClass('ajax_add_to_cart');
	  	}

		$(document).find('iframe[src*="youtube.com"]').each(function() {
            var td_video = $(this);
            td_video.attr('width', '100%');
            var td_video_width = td_video.width();
            td_video.css('height', td_video_width * 0.5625, 'important');
        });
    });

})( jQuery );
